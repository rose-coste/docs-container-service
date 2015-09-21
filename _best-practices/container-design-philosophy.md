---
title: Container design philosophy
slug: container-design-philosophy
description: Best practices for container design, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Container design philosophy
===========================

As a general rule of thumb, the author believes that authoritative
decisions from a design and development standpoint come directly from
Docker Inc. themselves, primarily from Docker CTO Solomon Hykes. Though there
are several voices in the container industry, Hykes' opinions around
suggestions tend to be well received. The relationship between Solomon Hykes
and the Docker community is similar to that of Linus Torvalds with the
Linux Kernel community. However, since Docker Inc. is in fact a business,
one can expect that Hykes has a responsibility to his investors and
himself to position all of Docker’s offerings as the preferred
methodology – as one can imagine in these scenarios, the suggestions
should be taken with a grain of salt.

According to Docker’s “Best Practices” guide, it is suggested that you
“run only one process per container (26).” Though it initially seems as a
simple design decision that you could easily overlook, this concept has
been met with various opinions,
discussions and interpretation in the community.

Though you can
*actually* run more than one process per Docker container, some believe
that this diverges away from the simple packaging and ease-of-use that
containers are supposed to provide. Confusion about this philosophy stems
primarily from newbies wanting to find a relatable manner to use
containers: their natural instinct is to treat it as a VM that they
would SSH into, hence the necessity for multiple processes. However,
once one realizes that there are other methods of working with a
container (such as enforcing proper logs and using development containers with
a TTY for shell interaction) this necessity and/or hurdle of
packing many different processes into a container is no longer needed.

When this insight is made, the offering that containers provide really
shines through and the instinct to create monolithic packages, such as
one would do with a VM, becomes less relevant. Thus, the adoption of a
microservice architecture becomes prominent as its relationship to
containers not only appears as a better and clearer fit, but a concept
that is now synonymous with using containers.

Martin Fowler describes the concept in this way:

> The term "Microservice Architecture" has sprung up over the last few
> years to describe a particular way of designing software applications
> as suites of independently deployable services. While there is no
> precise definition of this architectural style, there are certain
> common characteristics around organization around business capability,
> automated deployment, intelligence in the endpoints, and decentralized
> control of languages and data (27).

If one is willing to accept this concept when using containers, then
it is safe to state that microservices are emerging as the pioneered
methodology when it comes to building your application’s stack. It
evangelizes software to not only become more modular, but it
intrinsically advocates for loose coupling of dependencies, which aid
in alleviating the pain points of CI/CD, and ultimately, aid in
creating better software.

In closing, it is easier from both a mental and technical
standpoint that one should manage and use Docker containers as
roles such as an app, database, cache layer, load balancer etc. rather
than as individual processes (nginx, apache2, sshd etc.) – doing so,
will highlight the strengths of a container + microservice
architecture for your application’s stack.
