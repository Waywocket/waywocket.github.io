# My Goals for a CI Workflow

## Feature Requirements (WIP)
At the end of the day, a team has to make a lot of decisions about the specific
behavior of their automation. Thrashplay is no different. For *origination*
phase projects, our CI requirements are:

- Build + Test + Etc.
- No Publish Commits
- Tag things
- publish stuff?
- Etc.

## Philosophy

If you are interested in the philosophy and fluffy "why's" behind these 
requirements, you should check out the page on [my CI philosophy](./philosophy)

The feature requirements for my CI workflow arose out of some philosophical 
goals I had in mind when creating it. These are fluffier than the technical 
requirements, but might be useful in helping other teams figure out what they
could be thinking about.

The "soft" goals of our CI system are that it:
- Provides basic CI functionality
- Uses git politely
- Enforces good habits

### Provides basic CI functionality
> Sneak peek: [Drone Configuration Reference](https://docker-runner.docs.drone.io/configuration/)
> and [Plugin Registry](http://plugins.drone.io/)
{: .section-intro}

Obviously, as a continuous integration solution, continuous integration features
were the top priority. These include:

- Each commit triggers a build
- Build steps are easily configurable (lint, compile, test, etc.)
- Stakeholders are notified of build status and results 
- *etc.*

These features are basic CI capabilities, and Drone handles all this
and much more. Git integration, build command execution, and notifications are 
supported by a robust suite of plugins and configuration options. 

The hard part here is going to be creating all the specific build tasks for your 
project: What are the lint rules? How is your source code structured? *Writing unit
tests...*

### Uses git politely
The CI system should annotate the git history such that builds can be repeated 
and versioned artifacts can be reproduced on-demand. It should do this without creating
noise or hampering developers. Specifically:

- Every (successful) build should be tagged in predictable manner
- Useful and/or necessary metadata should be added to source control
   - Update versions in project configuration files (i.e. `package.json`)
   - Create changelogs from [conventional commit](https://www.conventionalcommits.org/en/v1.0.0/) messages
   - Associate commits with build numbers for later reference
   - Replace issue numbers with full issue tracking links in commit logs
- Commit history should not be polluted
   - Many CI systems create 'publish' commits for each build. I hate these.

### Enforces good habits
Developers shouldn't have to perform any work to do the right thing. *Zero*. 
Seriously, those guys are lazy. Every commit and every build should adhere to
every requirement of the system, every time. Enforcing lint rules and commit
message requirements, validating test coverage, recording changelog notes,
bumping package versions in a prescribed manner. Once a project's requirements 
are identified, they should be baked into the CI and automated mercilessly.

