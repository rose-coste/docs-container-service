---
title: Container ecosystem: Mesos versus OpenStack
slug: container-ecosystem-mesos-openstack
description: Best practices for container ecosystems, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Container ecosystem: Mesos versus OpenStack

With Docker’s rise in fame, the ecosystem built on top of it as well as
the one that empowers it has introduced technologies, both new and
existing, to be considered as options in the stack. Mesos is one of the
technologies that have gotten traction in the ecosystem, especially with
its recent support of Docker containers and the development of
Mesosphere that orchestrates containers on top of Mesos. However, there
is confusion about whether one should be integrating Mesos in to their
stack or be using OpenStack, or even some other technology to empower
the PaaS offering from the infrastructure layer.

In short, people tend to think of Mesos as a competitor to OpenStack, or
vice versa; however, it’s quite comparable to the apples versus oranges
debate. In fact, one can run Mesos on top of OpenStack, and this tends
to be a very common operating model, but if you choose to, you can also
just run Mesos directly on bare metal.

The differentiating factor about Mesos and OpenStack is that OpenStack
splits up your cluster across several VM’s for your applications to run
on, and Mesos
combines all of your resources, VM or bare metal, and presents them as a
single entity or machine; therefore, one could think of Mesos as one
very large machine to run your applications.

The promoters of Mesos argue that VM’s were meant to solve the issue of
consolidating several, differentiating workloads on far fewer machines
than they once needed, ultimately saving money, management of resources
and moving away from traditional virtualization which was plagued with
horrible turnarounds and bottlenecks – this much is true. In contrast,
Mesos to a degree reverses this pattern at the cost of removing full
isolation for your application due to the nature of how Linux containers
operate versus VM’s.

In particular, recent attention to Mesos in the Docker ecosystem centers
around the concept that greenfield projects are looking to create more
of their applications in containers rather than VM’s, so designing your
infrastructure to run and consume OpenStack seems like overkill and an
added complexity when you can achieve the same efficiency from Mesos.

At the end of the day, it really comes down to necessity and planning
for the future infrastructure that your applications will consume.
Whether that is Mesos, OpenStack, or a mix of the two, will highly
depend on how decoupled and/or separate you want to create your
datacenter, stack and application to accommodate the nature of varying
workloads from different user bases whether it be in VMs, containers,
or both.

## Resources

For detailed background information about the ecosystem of container technologies
for the Rackspace Container Service,
follow the links suggested at
[Introduction to container ecosystems and technologies: suggested reading]
(/container-technologies-references/).
