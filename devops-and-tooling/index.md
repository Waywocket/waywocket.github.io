---
image:
  src: https://imgs.xkcd.com/comics/automation.png
  caption: 'Source: <a href="https://xkcd.com/1319/">xkcd</a>, used <a href="https://xkcd.com/license.html">with permission</a>'
---
# Operations and Tooling
I should warn you that I fall for the trap pictured in this *xkcd* comic a lot.
So you should probably take that into account when listening to any advice in
this section.
{: .page-intro}

I have, over the last couple years, gained a lot of interest in DevOps &mdash; by
which I mean developing software in a way that integrates traditional 
development tasks with traditional operations tasks. Hopefully in a way that is
both effective and highly agile.

This interest, combined with a need to deploy *stuff* on projects without an 
operations staff, has given me an opportunity to play with a lot of tools. This
site is a platform for me to share some of that meager knowledge with the wider 
world.

Eventually I would like to talk about a number of things here, assuming I ever
get around to writing the content. For now, hopefully something in the following
list is interesting to you.

## [How I Use Drone for CI](./continuous-integration/)
Drone is an easy-to-setup and flexible continuous integration server that uses 
Docker images to define build steps and plugins. And it's free. I have created 
a pretty cool (in my opinion) CI workflow for my own projects using Drone, and
hope that [my experience doing so](./continuous-integration/) is useful
to someone else.

