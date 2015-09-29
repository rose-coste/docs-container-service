---
title: Container ecosystem: Docker Swarm
slug: container-ecosystem-docker-swarm
description: Best practices for container ecosystems, powered by the Rackspace Container Service
author: Mike Metral, Rackspace Product Architect
topics:
  - docker
  - beginner
---

## Container ecosystem: Docker Swarm

A quick Google search on Docker Swarm primarily reveals sites discussing
its announcement, but blogs, articles, and community forums centered on
reviews and/or experience with it are almost non-existent compared to
other container orchestrating tools. Docker founder Solomon Hykes
said this about Docker Swarm:

> “Fig [now known as Docker Compose], Kubernetes, Mesos etc. are
> competing orchestration tools. Docker will give [developers] a standard
> interface to all three…[and Swarm, formerly known as libswarm]…is an
> ingredient of that [standard] interface, [specifically, acting as] the
> glue between Docker and orchestration backends [(1)](#resources)”.

If you’re taken aback or confused by this explanation of what Swarm’s
added benefit is,
you are not alone. Swarm attempts to offer entry-level
orchestration for managing distributed containers, but whenever you’re
ready to adopt another orchestrator that aligns with your goals for
scale, you can easily swap out Swarm’s engine for the one of your
choice. For some ideas about alternative orchestration tools, read
[Introduction to container technologies: orchestration and management of container clusters]
(/container-technologies-orchestration-clusters).

It is the author’s opinion that the strategy behind Swarm is that
Docker did not want to get into the orchestration battle that is
taking place between frontrunners Kubernetes and Mesosphere (which
runs on top of Mesos). Instead, Docker wanted to use Swarm to extend their
command line interface, to an extent, but also be able to interact with the
other orchestrating engines. This tactic seems to be one that
keeps Docker relevant in terms of managing containers, local or
distributed, and possibly a vector that Docker plans on productizing
and monetizing from in some form in the future, so long as the user
doesn’t switch directly to the orchestrating backend.

However, the community hasn’t really shown much
interest in Swarm, and is choosing to consume, learn, interact and
design their stacks for their orchestrator of choice directly. What
the future holds for Swarm is still to be determined, but the gist of
it today does not seem to be offering anything worth adopting.

<a name="resources"></a>
## Resources

1. <https://twitter.com/solomonstre/status/492111054839615488>

- [Introduction to container technologies: orchestration and management of container clusters]
(/container-technologies-orchestration-clusters)
- <https://docs.docker.com/swarm/>

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.
