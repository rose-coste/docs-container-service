---
title: Container ecosystem: Kubernetes
slug: container-ecosystem-kubernetes
description: Best practices for container ecosystems, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Container ecosystem: Kubernetes
===============================

Community Status
----------------

As detailed in “State of the Union: Containers – Part 1,” Kubernetes
is still the front-runner for how to manage and orchestrate
containers in your stack. Though it is still in beta and not
production-ready yet, the industry has not strayed away from betting
on Kubernetes’ future and success, solely based on the fact that
Google is backing it and the quality and types of contributors working
on it are impressive.

Aside from the positive blog posts and community chatter influencing
its adoption, the community is showing its vested interest:
as of April 2015, Kubernetes averaged around 400-500 commits per
week and had almost 300 contributors – a very substantial following.

Operating Model
---------------

There is lots of material covering what Kubernetes’ technical
capabilities are and what its purpose is, however, we want to take the
opportunity to take a step back and give a synopsis of how Kubernetes
is intended to be used and where the real purpose on including it in
your stack lies.

Kubernetes’ added benefit is that it defined a collection of
primitives to aid in establishing and maintaining a cluster of
containers – in short, Kubernetes is really just an opinionated model
for application containers, their dependencies with regards to other
resources, and its lifecycle. Without going into too much fine-grained detail,
*pods* “are the atom of scheduling, and are a group of
containers that
are scheduled onto the same host…[that] facilitate data sharing and
communication [by way of] shared mount point’s, [and] network namespace
[to create] microservices. (34)”

The rest of the concepts/units Kubernetes outlines such as *Services*,
*Labels* and *Replication Controllers* are ways to enhance *Pods*, as
well as serve as a method for the user to declare the intended state
their containers should hold and have Kubernetes enforce. Therefore, it
is safe to say that Kubernetes does not even know what an application or
microservice actually is, just how you wish to collect and manage the
containers, including the adherence of requirements such as resource
allocation for containers, affinity, replication, load balancing etc. –
anything more than that is beyond the scope of Kubernetes.

Kubernetes-specific functionality
---------------------------------

Kubernetes does not directly intend to reinvent core Docker
functionality, but it does establish its own slightly divergent
semantics for concepts such as volumes and container communication etc.,
an in some cases, it does have its own implementation of the concept.

#### Volumes

With regards to Docker volumes, we’ve learned in this report about Data
volumes and Data volume containers. With Data volumes, you can either
let Docker choose a random, unspecified location for you to create the
volume on the host or you can specify a host directory/file to mount on
the container. In Kubernetes, new abstractions such as volume and volume
mounts exist for usage with pods with volume types known as EmptyDir and
HostDir. However, behind the scenes, these types essentially map to the
way one can use a Docker Data volume, with additional overhead for
Kubernetes to maintain the user’s configuration while it interfaces
directly with Docker to actually enable the sharing of volumes.

Where Kubernetes does differ from Docker with regards to volumes is in
Data volume containers. Docker, as well as various blog posts and articles
prescribe this mechanism as the preferred way to share data amongst
containers. As we’ve seen in the previous section on Docker volumes,
various intrinsic issues exist when you begin to scale out your
architecture, particularly when using and depending on Data volume
containers. The folks behind Kubernetes have recognized this methodology
as a potential for failure and have chosen to not support Data volume
containers as a type in their volumes. The reasoning being that Data
volume containers are ultimately passive containers that cannot only be
very unintuitive to understand from a user perspective, but could create
corner cases and potentially be problematic for management systems.

#### Discovery

For container service discovery, specifically in a pod, Kubernetes does
not directly use Docker links as they don’t do well outside of a single
host and managing their lifecycle can prove to be difficult given its
current capabilities. Instead, Kubernetes offers two modes of discovery
for the concept of *Services* that resemble linking: environmental
variables and DNS.

If a Kubernetes *Service* exists, then Kubernetes has a backward
compatible mannerism to create Docker-style link environment variables
in the container as described in the section on “Container Linking,” but
remember that we believe they’re implicit and hard to work with. It can
also create simplified environmental variables with the pattern
{SVCNAME}\_SERVICE\_HOST and
{SVCNAME}\_SERVICE\_PORT.

Another recommended way to perform service discovery is by using a DNS
server. The DNS server watches the Kubernetes API for new *Services* and
creates a set of DNS records for each. If DNS has been enabled
throughout the cluster then
all *Pods* should be able to do name resolution of *Services*
automatically (35). The takeaway from this is that you don’t need to
explicitly create links between communicating pods as done natively in
Docker because Kubernetes does the heavy lifting, so long as you oblige
by the networking mechanisms described in the proceeding section.

Lastly, lets not forget that 3rd party tools such as etcd, Zookeeper and
Consul are also viable options.

#### Networking

Kubernetes deviates from the default Docker networking model. The goal
of Kubernetes networking model is to allow each pod to have an IP in a
flat networking space in which it can communicate with hosts and
containers across the cluster. In doing so, pods can be thought of as
any other node in the network with regards to “port management,
networking, naming, service discovery, load balancing, application
configuration, and migration” and can create a NAT-free address space
– this concept is known as the “IP-per-pod” model (36).

Because Kubernetes applies IP addresses at the *Pod* scope -­‐
containers within
a Pod share their network namespaces, including their IP address.
This means that
containers within a pod can all reach each other’s ports on *localhost*.
This does imply that containers within a Pod must coordinate port usage,
but this is no different that processes in a VM (37).

We can achieve the IP-per-pod model via the prescribed network
requirements imposed by Kubernetes by allocating each host (minion) with
its own subnet in an overlay network that can enable containers to
communicate with the host and any other networks available in the
enviromnet. A common network split is to allocate the overlay a
cluster-wide /16 and then divvy up a /24 for each minion. Once you
lay out your network space, you can implement the overlay and configure a
new bridge for the Docker host to use within it. Some tools that are
great for this particular purpose, especially in a cloud environment,
are container-intended networking technologies such as Flannel, Weave,
SocketPlane and even Open vSwitch.

This is different from the standard Docker model. In that mode, each
container gets an IP in a host-private networking space defined in
RFC1918 (such as in the 172-dot space) managed by the Docker host
bridge. The effect of this is that containers can only communicate with
other containers on the same host, as opposed to also being able to
communicate with other machines in the network as well. Furthermore, it
may even be the case that conflicts and confusion arise due to different
Docker hosts using the same network space and configuration.
