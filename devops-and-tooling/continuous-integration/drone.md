# Continuous Integration Using Drone




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