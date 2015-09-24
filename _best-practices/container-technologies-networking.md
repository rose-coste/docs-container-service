---
title: Introduction to container technologies: container networking
slug: container-technologies-networking
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Introduction to container technologies: container networking

Containers become useful when they are connected to each other and to the rest
of your network. Several tools can help you establish and maintain a network of
containers.

## Weaveworks “Weave”

Weave "makes the network fit the application, not the other way round,"
as Weave co-founder Alexis Richardson puts it.
With Weave, Docker containers are all part
of a virtual network switch no matter where they are running. Services can
be selectively exposed across the network to the outside world through
firewalls and using encryption for wide-area connections (20).

When using Weave, “applications use the network just as if the containers
were all plugged into the same network switch, with no need to configure
port mappings, links, etc. Services provided by application containers
on the weave network can be made accessible to the outside world,
regardless of where those containers are running. Similarly, existing
internal systems can be exposed to application containers irrespective of
their location (21).”

Richardson also stated that "weave establishes per application
Layer 2 networks for containers across hosts, even across cloud providers
and other seemingly complex cases with minimum fuss (22)." Add this
functionality to the fact
that Weaveworks recently raised five million dollars in
Series A venture capital funding, and it makes a compelling argument to consider
Weave as viable option.

## CoreOS’s “Flannel”

Flannel is positioned as CoreOS’ primary way to manage container
networking via a private mesh network for the containers in a cluster,
which happens to do away with issues such as port mapping.

At its core, Flannel is an overlay network that provides a subnet to
each machine.
This is the main operating model that Kubernetes prescribes for all
minions and nodes hosting containers. Flannel is backed by and based on CoreOS’
etcd to serve as the key/value store for the networking configuration and
state management. Though Flannel was originally intended for Kubernetes, it has
evolved into a generic overlay.

Flannel is still in its early stages and development is very much in
flux and somewhat happens in spurts. It should be perceived as
experimental. However, don’t disregard Flannel’s presence in the market, as
their roadmap looks very optimistic given that CoreOS plans to be a big player in
the container space.

## Metaswitch’s “Calico”

Project Calico “integrates seamlessly with the cloud orchestration
system (such as OpenStack) to enable secure IP communication between
virtual machines. As VMs are created or destroyed, their IP addresses are
advertised to the rest of the network and they are able to send/receive data
over IP just as they would with the native networking implementation – but with
higher security, scalability and performance (23).”

In late 2014, the Calico team managed to create a prototype of the Calico stack
that runs as Docker containers, in addition to a plugin that informs it
of all containers in the system. This prototype established as a proof
of concept that the networking model that Calico enables does work for containers.

Though the team seems to have some ideas as to how to proceed with
Calico and Docker, there are no short-term plans to evolve the prototype.
This has put the drive and initiative to do so into the hands of the
community.

## SocketPlane’s “SocketPlane”

Socketplane’s concept is to bring Open vSwitch to the Docker host so that one can
“have a container that’s going to be able to manage the data path and
also manage either overlays or underlays (24).”

However, if one were to look for an actual project to evaluate or even
their webpage, you’ll be met with neither as Socketplane is still very
much in a semi-stealth mode. Its relevance and consideration as an
option stems from the fact that its founders, who left RedHat to start Socketplane,
are three very well known
networking gurus who are also contributors to the OpenDaylight Project.
The team currently consists of Madhu
Venugopal, Brent Salisbury, and Dave Tucker.

It is expected that a SocketPlane product is going to be made available
sometime in
2015, so with both the concept and the team behind it, this could evolve
into a sound and promising technology. It has recently been made public
that SocketPlane was purchased by Docker, Inc and they plan to natively
integrate with the Docker ,Inc portfolio (25).

## Comparison

It is very early in the Docker ecosystem to tell which
container-networking solution will prevail, let alone which tools are
being used a production scale, as this space is quite new. Though
intriguing and backed by some powerful teams, Flannel, Calico for
Docker, and SocketPlane show signs that either not enough attention is
being given to the project or there have not been any concrete products to
seriously evaluate and test.

**Current Recommendation** Weave (based on project attention, evolution
and funding)

## Resources

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.