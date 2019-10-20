# Continuous Integration
  Setting up a robust continuous integration pipeline is hard. Like, really hard.
  "People do this as their full-time careers" hard. In this section I try to make
  it at least a little easier for the solo developer or small team.
  {: .page-intro}

A well-functioning process to automate the build and deployment of software is
crucial to developing that software rapidly. It's like your commuter car -- 
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
