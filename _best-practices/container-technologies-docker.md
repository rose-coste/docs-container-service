---
title: Introduction to container technologies: Docker
slug: container-technologies-docker
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Introduction to container technologies: Docker

Docker is an open-source project primarily focused on automating the deployment
of applications within containers.

## Background

Container technology has been around for many years but its popularity
increased along with the popularity of Docker.

Docker was released in early 2013 by Docker Inc., formerly known as
dotCloud. Docker has benefited from a massive amount of community development
contribution and has amassed an immense following from people in various
roles such as developers, devops, CIOs and CTO’s.

The success of the Docker project has even has gone as far to accumulate
several millions of dollars in funding for Docker Inc. providing them
with an estimated valuation of between 200 and 400 million dollars at the time of
this report (2).

Docker and the ecosystem being built around it,
primarily the container orchestration and management tools, are providing
a popular and open-source means for companies of all sizes to adapt
to the scalable, dynamic, and agile technologies that similarly power the
infrastructure and applications at tech giants such as Facebook, Google
and Amazon. You can read more about the ecosystem of orchestration tools
for containers at
[Introduction to container technologies: orchestration and management of container clusters]
(/container-technologies-orchestration-clusters/).

# Overview

Docker is based on Linux Containers (LXC), which is a simplified
virtualization environment that allows you to run multiple, isolated
Linux systems on a single Linux host.

In a traditional virtual machine, the instance is allocated its own set of resources that
allows for true isolation on the host, but it comes with a heavy price in
terms of the resources required to run it. Another problem is the lack of
portability of the virtual machine across different virtualization platforms:
think
of the difficulty of trying to share a virtual machine image across
Amazon AWS, Rackspace Cloud, Microsoft Azure and VirtualBox. Now think
about the size of the image in addition to the image format, and how
transferring it from one platform to the other doesn’t always prove to
be the most time- and cost-effective. These two issues just begin to
scratch the surface of the difficulties of managing virtual machines in
complex environments today.

In a container, you experience less isolation from other containers running
on the same host, which each container has a much smaller resource footprint
than a comparable conventional virtual machine.
This allows for far more usage of your resources, and because because they’re
much lighter they can be instantiated much more quickly than non-containerized
virtual machines.

Unfortunately, the one key functionality that the hypervisor provides today
that a container does not is that you can co-locate different operating
systems or kernels on the same platform. Therefore, if you have decided to run
instances of Windows alongside instances of Debian, Ubuntu, or Red Hat,
then containers aren’t applicable to your use case. This is due to the fact
that hypervisors abstract an entire machine whereas containers only
abstract the physical host’s operating system kernel. Nevertheless, as of
October 2014 Microsoft and Docker announced a partnership to invest in
enabling the Windows Server container in the Docker engine, in addition to
other Docker support in the suite of Microsoft products at a future date (3).

In its simplest form, you can think of Docker as a wrapper
for LXC. However, Docker provides much more functionality through a layer
of abstraction and automation of LXC, as well as a high-level API and a
booming ecosystem that is actively being built around it.

Docker’s main features center on:

* providing ease of portability through
  its format and bundling capabilities
* being optimized for the deployment
  of applications rather than machines
* a philosophy that components, including containers themselves,
  should be reusable as
  base images for other containers

In addition to the core features of Docker, sharing Docker images through
what is known as an image repository allows for the communal use of
applications. These applications can be pre-fabricated and uploaded by
others to facilitate their consumption. For example, one can easily
download a Docker image and in a matter of seconds, run in that container
the latest MySQL
Server, Redis, Apache, Golang Environment, or OpenVPN Server without
having to go through the full setup and configuration process for each
tool, let alone worrying whether it will function on your virtualization platform.

These benefits are possible because Docker provides the ability to easily
and quickly snapshot your application and its operating system components into a
common image that can be
deployed on other hosts that also run the Docker engine. This capability
resonates in the technical community that is unfortunately familiar
with a sea of different virtual machine image formats and hypervisors that don’t
play well together without some heavy lifting, if at all.

In short, Docker is about being able to consistently deploy an
application environment in an easy and reproducible manner. In doing so,
Docker has started a forward progression of IT infrastructure and how we
construct the applications that run on the infrastructure to truly be
more flexible in
various aspects than ever before.

## Basic concepts

-    **Container**: The technology that allows the deployment and
     execution of an application and includes its dependencies, user files
     settings and the operating system.

-    **Docker Daemon/Engine/Server:** Responsible for managing and
     instantiating the containers.

-    **Docker Command-­‐line Client:** Allows a user to communicate and
     control the Docker server daemon.

-    **Dockerfile**: Dockerfile is a set of instructions to run over a
     base image. Docker followes this instructions to build an image.

-    **Docker Image**: Blueprint or template used to launch the Docker
     container.

-     **Docker Index / Image Repository:** Registry of Docker images that
      can be browsed and downloaded. The public Docker image repository is
      located at <https://registry.hub.docker.com> but a private, local version
      of the repository can be run if needed.

## Operational concepts

Recall that containers share a common operating system kernel.
The following concepts allow each container to
be lightweight and fast while
having some form of isolation and
proper resource allocation

-    **cgroups (container groups)**: Kernel feature which accounts for
     and isolates the resource usage (CPU, memory, disk, I/O, network)
     of a collection of processes (4).

-    **namespaces**: Kernel feature that allows for a group of processes
     to be separated such that they cannot see resources in other groups (5).

-    **unionfs**: Filesystem service which allows for actions to be done
     to base image. By this method, layers are created and documented, such
     that each layer fully describes how to recreate a action. This
     strategy enables Docker's lightweight images, as only layer updates
     need to be propagated
     from one environment to another, much like the code repository system
     git operates.

## Current container philosophy

Since Docker and the Docker ecosystem is currently a emerging field, it is
worth noting that containers are not intended to be a replacement for
virtual machines or baremetal machines, let alone be applied to all use cases.
Containers are certainly fulfilling a solution to the painful problem
for developers, operations and devops with regard to the
lifecycle of apps and managing their environments, but a greenfield stance
must be accepted and held first and foremost.

Also, there is a lot of competition and overlap across several of the
options and its hard to pinpoint which is better than the other. That
being said, not all use cases can be adapted perfectly into containers
today, particularly, stateful applications, but work in this space is
being addressed by several technologies and should be in a much better
state at a future date.

This report intends to educate and outline recommendations of which
technologies are currently the best option, but there is no simple answer.
All of
these tools are not only young, but are also moving at a rapid pace
and the scale and ease at which they clearly outline what they’re meant
to aid with, let alone which technologies they respectfully tend to
interoperate with, is still to be determined. It is the author’s opinion
that throughout 2015, the community will make it known which
technologies are meant to stick and which should be shelved.

## Resources

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.
