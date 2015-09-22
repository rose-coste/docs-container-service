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

## Weaveworks “Weave”

Weave "makes the network fit the application, not the other way round,"
as the company CEO puts it. With Weave, Docker containers are all part
of a virtual network switch no matter where they're running. Services can
be selectively exposed across the network to the outside world through
firewalls and using encryption for wide-area connections (20).

When using Weave, “applications use the network just as if the containers
were all plugged into the same network switch, with no need to configure
port mappings, links, etc. Services provided by application containers
on the weave network can be made accessible to the outside world,
regardless of where those containers are running. Similarly, existing
internal systems can be exposed to application containers irrespective of
their location (21).”

Alexis Richardson, CEO, stated that "weave establishes per application
Layer 2 networks for containers across hosts, even across cloud providers
and other seemingly complex cases with minimum fuss (22)." Add to the fact
that they just raised
\$5M in Series A, and it makes a compelling argument to considerably
evaluate Weave as viable option.

## CoreOS’s “Flannel”

Flannel is positioned as CoreOS’ primary way to manage container
networking via a private mesh network for the containers in a cluster,
which happens to do away with issues such as port mapping.

At its core, Flannel is an overlay network that provides a subnet to
each machine that initially was intended for Google’s Kubernetes, as
this is the main operating model that Kubernetes prescribes of all
minions/nodes hosting containers. Flannel is backed and based on CoreOS’
etcd to serve as the key/value store for the networking configuration and
state management. Though it was originally intended for Kubernetes, it has
evolved into a generic overlay.

Flannel is still in its early stages and development is very much in
flux and somewhat happens in spurts. It should be perceived as
experimental but don’t disregard Flannel’s presence in the market, as
their roadmap looks very optimistic given that CoreOS plans to be a big player in
the space.

## Metaswitch’s “Calico”

Project Calico “integrates seamlessly with the cloud orchestration
system (such as OpenStack) to enable secure IP communication between
virtual machines. As VMs are created or destroyed, their IP addresses are
advertised to the rest of the network and they are able to send/receive data
over IP just as they would with the native networking implementation – but with
higher security, scalability and performance (23).”

In late 2014, the team managed to create a prototype of the Calico stack
that runs as Docker containers, in addition to a plugin, that informs it
of all containers in the system. This prototype has established that the
networking model Calico enables does work for containers as far as a proof
of concept.

Though the team seems to have some ideas as to how to proceed with
Calico and Docker, there are no short term plans to evolve the prototype
and has put the drive and initiative in doing so, into the hands of the
community.

## SocketPlane’s “SocketPlane”

Socketplane’s concept is to bring Open vSwitch to th Docker host so that one can
“have a container that’s going to be able to manage the data path and
also manage either overlays or underlays (24).”

However, if one were to look for an actual project to evaluate or even
their webpage, you’ll be met with neither as Socketplane is still very
much in a semi-­‐ stealth mode. Its relevance and consideration as an
option stems from the fact that its founders are three very well known
networking gurus as well as contributors on the OpenDaylight Project that
left RedHat to start Socketplane. The team currently consists of Madhu
Venugopal, Brent Salisbury and Dave Tucker.

It is expected that a product is going to be made available in early
2015, so with both the concept and the team behind it, this could evolve
into a sound and promising technology. It has recently been made public
that SocketPlane was purchased by Docker Inc and they plan to natively
integrate with the Docker Inc portfolio (25).

## Comparison

It is very early in the Docker ecosystem to tell which
container-­‐networking solution will prevail, let alone which are
being used a production scale, as this space is quite new. Though
intriguing and backed by some powerful teams, Flannel, Calico for
Docker, and SocketPlane show sign that either not enough attention is
being given to th project or there have not been any concrete products to
seriously evaluate and test.

**Current Recommendation** Weave (based on project attention, evolution
and funding)

## Resources

For detailed background information about the ecosystem of container technologies
for the Rackspace Container Service,
follow the links suggested at
[Introduction to container ecosystems and technologies: suggested reading]
(/container-technologies-references/).
