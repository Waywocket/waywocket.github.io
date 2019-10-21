---
references:
  - title: "Continuous Integration (ThoughtWorks)"
    url: https://www.thoughtworks.com/continuous-integration
  - title: "Drone Installation: Overview"
    url: https://docs.drone.io/installation/overview/
  - title: "Drone Pipelines: Overview"
    url: https://docs.drone.io/configure/pipeline/overview/
  - title: "Git Documentation: --interpret-trailers"
    url: https://git-scm.com/docs/git-interpret-trailers
  - title: Semantic Versioning 2.0.0
    url: https://semver.org/spec/v2.0.0.html
---
# What We're Building
See it in action: [Drone Feed for thrashplay-app-creators](https://drone.thrashplay.com/thrashplay/thrashplay-app-creators)
{: .lead}

This section of *Type Harder!* is dedicated to detailing how to build a 
continuous integration system like the one I use for all of my projects.
The system I describe here can be used as-is to continuously deliver npm
packages from a monorepo to the public npm registry. It uses a workflow I 
decided on for projects in what I call the [*origination* phase](/glossary). 

The system detailed here is a solid foundation for future customizations and
enhancements, such as continuous delivery of applications (web, mobile, or
other) instead of simple library packages.

## Services and Components
These walkthroughs will guide you through how to build, download, install, 
and/or configure several components. At a high-level, this process will 
involve creating the following pieces:

* An SQL database for storing Drone's configuration and logs
* A Drone server for controlling your CI system
* One Drone runner for executing build tasks
* A project-specific Drone pipeline configuration (*.drone.yml*)

I have created a high-level conceptual diagram of the relationship between 
the Drone server, Drone runners, and your source code &mdash; all of which
are discussed in greater depth below:

[![]({{ '/assets/images/continuous-integration/drone-architecture-simple.svg' | relative_url }} 'Architecture diagram showing how the Drone server dispatches jobs to a Drone runner.')]({{ '/assets/images/continuous-integration/drone-architecture-simple.svg' | relative_url }}){:target="_blank"}

### Drone Database
Drone uses an SQL database to store configuration, job history, and logs. The
developers recommend PostgreSQL, but MySQL is also supported. In practice, I
have used a Google Cloud MySQL instance with no issues.  While this database is
technically an optional dependency, without it you will lose all job history,
user configuration, and project configuration any time the Drone server is
restarted. This is fine for initial testing, but for any 'real' work you will
want a database.

I will not be detailing the setup and creation of this database, but will 
assume you have the necessary connection information and credentials when the
time comes to configure it.

### Drone Server
The Drone server is the central component of the Drone CI system. It provides
a dashboard for configuration and monitoring of the system, receives webhook
calls from the various source control management systems containing your 
projects, and dispatches jobs to the *runners* that perform the work of 
building your code.

The server is distributed as a Docker image that has no (mandatory) external
dependencies. As such, it can be run in any server environment capable of
hosting Docker containers. I will walk you through the process of deploying
the Drone server to Kubernetes &mdash; specifically, Google's Kubernetes
Engine (GKE), since that is what I use to host my Drone server.

### Drone Runners
Drone allows runners to be deployed on many servers in order to create a
distributed build network. For my current setup I have a single Docker-based
runner hosted in the same GKE cluster as my Drone server, and so this is
the configuration I will describe in this article. However, Drone is 
flexible enough to easily support much more complicated setups should you
need it.

### Your Project
Obviously, I am assuming you have one or more projects in mind that you would
like to have continuously built and published. I will not delve too deeply
into setting up your project, beyond the following:

* Basic [Lerna](https://github.com/lerna/lerna) setup and configuration, for monorepo versioning
* How to execute build commands. We will use Yarn, but anything from Makefiles to Maven should work
* Creating Drone pipeline configurations, using Yaml or JSonnet

This last item will be where we spend most of our time, as the first two items
are not really unique to the task at hand.

## Pipelines
*Pipeline* is a fancy CI/CD term for the ordered steps (workflow) your code undergoes
from initial check-in to final deployment. Drone's documentation simply says
that *"a pipeline defines the Continuous Integration and Delivery process for
your project."*

In Drone, a project can have multiple different pipelines configured, each
associated with a different *trigger* &mdash; such as 'when code is pushed' or
'when a pull request is opened'. These different pipelines then define the
workflow that should be executed on the code when that trigger occurs.

The system that we are going to create is built on three pipelines:

* A `build` pipeline, for continuous integration of code changes
* A `publish` pipeline, for pushing pre-release packages to a registry
* A `promotion` pipeline, for graduating pre-releases to stable versions

### Build Pipeline
The `build` pipeline receives webhook notifications when code is pushed to one
or more monitored GitHub repositories. These webhooks trigger the following
steps:

1. Checkout the code, and run a configurable set of build steps
  * We will be using Yarn commands to build and test npm packages
  * Anything that can be run in a Docker container can be part of the build, and many plugins exist for common requirements
1. Amend the commit with additional metadata, including:
  * Update the project version number (in `package.json`, `pom.xml`, etc.). We will be using pre-release versions (per the [semantic versioning 2.0.0 spec](https://semver.org/spec/v2.0.0.html)) in our automated builds
  * Append the new changes to a project Changelog
  * Add build # or other info to [trailers](https://git-scm.com/docs/git-interpret-trailers) in the commit message
1. Tag the commit in git to indicate a successful code version that passes tests and is a candidate for release

**Note:** The process outlined above *amends existing commits*. This means
the CI system will (1) alter commits made by developers in an
automated fashion and (2) force-push those changes to the repository.
{: .alert .alert-info }

I use very specific `--force-with-lease` options to make this safe, but I understand
some people may not be comfortable with this workflow. If this describes you, you 
may still find a lot of valuable information in this section regarding how to setup
Drone and configure it for your project. You just may have to skip or modify 
some of the instructions around how project metadata and tags get pushed to git.

### Publish Pipeline
A separate `publish` pipeline receives webhook notifications when new tags are added 
to the project. This pipeline performs the following steps:

1. Publish the project's package(s) to the npm registry
2. If desired, custom `dist-tags` can be set on published packages to indicate the release channel (i.e. 'beta', 'next', etc.)

At a glance, the combination of `build` and `publish` pipelines looks something like 
the following illustration:

[![Thrashplay's CI Flow]({{ '/assets/images/continuous-integration/ci-workflow.svg' | relative_url }} 'Diagram showing the CI/CD steps we are going to build.'){: width="800px"}]({{ '/assets/images/continuous-integration/ci-workflow.svg' | relative_url }}){:target="_blank"}

### Promotion Pipeline
A third pipeline exists, as well, and is intended to be triggered manually 
(via the `drone` command line tool, a custom web UI, or integration with 
another tool). This pipeline triggers a 'promotion' build, which does the
following:

1. Upgrade the current, pre-release version to a stable version
2. Publish this version to the npm registry using the `latest` dist-tag

## What's Next?
On the next page, I am going to take [a brief detour](../philosophy/) to talk about why I decided
on this particular workflow, and why I chose Drone as my CI server of choice. 
If you don't really care about that, feel free to skip ahead to the technical
stuff in [Step 1: Deploy Drone](../deploy-drone/).



