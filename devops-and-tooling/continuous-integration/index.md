# Continuous Integration Using Drone
  Setting up a robust continuous integration pipeline is hard. Like, really hard.
  "Entire teams dedicate their entire full-time careers to this" hard. In this 
  section I try to make it at least a little easier.
  {: .topic-intro}

A well-functioning process to automate the build and deployment of software is
crucial to developing software rapidly. It's like your commuter car -- 
necessary to do your work, but absolutely taken for granted until it's not
working right.

I've spent way too many nights and weekends thinking about my ideal CI/CD 
process, and working to locate and/or customize and/or build the tools to make
it work. The key words in that sentence are "my ideal CI/CD" process. It's 
very likely I've made some choices you wouldn't have. But hopefully in reading
these pages, you will get some new ideas on what is possible.

## Final Product
> See it in action: [Drone Feed for thrashplay-app-creators](https://drone.thrashplay.com/thrashplay/thrashplay-app-creators)
{: .lead}

This section describes how to bootstrap a continuous delivery system to publish
npm packages to the public npm registry. It uses a workflow I decided on for
projects in what I call the [*origination* phase](/glossary). At a glance,
this workflow looks something like the following illustration:

[![Thrashplay's CI Flow]({{ '/assets/images/ci/ci-workflow.svg' | relative_url }}){: width="800px"}]({{ '/assets/images/ci/ci-workflow.svg' | relative_url }}) 

Some things aren't pictured (collisions caused by rapid commits, and promoting
builds from one release channel to another, for example). These topics will be
covered in later sections, however.

## Feature Requirements (WIP)
At the end of the day, a team has to make a lot of decisions about the specific
behavior of their automation. Thrashplay is no different. For *origination*
phase projects, our CI requirements are:

- Build + Test + Etc.
- No Publish Commits
- Tag things
- publish stuff?
- Etc.

If you are interested in the philosophy and fluffy "why's" behind these 
requirements, you should check out the page on [my CI philosophy](./philosophy)

## Prerequisites
In addition to what probably amounts to a lot of technical expertise, I assume
the readers of this guide have the following elements of their system already
in place:

- An environment to which Docker images can be deployed.
   - We used a [GKE Kubernetes cluster](https://cloud.google.com/kubernetes-engine/), but
     there are plenty of other environments that would work
- A MySQL or PostgreSQL database, accessible from the above
   - Drone supports both, but its developers recommend PostgreSQL. We use MySQL with no problems yet.
- Ability to create a git repository for each project. 
   - We use GitHub. Other hosts may require more or less Drone setup.

## Step-by-Step Guides (WIP)
The following guides will walk you through the entire process of transforming
the above prerequisites into a Continuous Integration system exactly liek the
one we are using. This probably isn't what you want, but along the way you will
hopefully learn enough to create the customizations that you *do* want.

1. (Bonus k8s Prerequisite) Use [Flux](https://github.com/fluxcd/flux)
2. Deploy Drone
3. Create a project
4. Do... something?