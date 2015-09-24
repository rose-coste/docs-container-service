---
title: Container ecosystem: OpenShift
slug: container-ecosystem-openshift
description: Best practices for container ecosystems, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Container ecosystem: OpenShift

RedHat’s OpenShift has always been associated with managing your
applications at the PaaS layer. With their three offerings--OpenShift
Online, OpenShift Enterprise, and OpenShift Origin--they aim to claim a
stake in various markets. Explicitly, Online competes with Heroku and
Google App Engine; Enterprise competes with CloudFoundry; and Origin is their
open-source attempt at contributing back to the community while
benefiting from the enhancements that open source brings to the table.

With Kubernetes really paving the way as the standard for container
orchestration in the Docker world, RedHat has taken the stance with
the next release of their products, version 3, to really focus, adopt
and build on Kubernetes. In doing so, RedHat is not only betting on
Kubernetes as the future of container orchestration, along with the
rest of the community, but they are allowing themselves to leverage
their Project Atomic, which is their preferred hosting platform for
Docker containers that competes with CoreOS.

RedHat has stated that OpenShift is capable of integrating with both
OpenStack and Project Atomic, but given the nature of RedHat’s
previous business models, the OpenStack integration is most likely
inline with the intent of OpenShift Origin: to appease the open source
community. It is quite the possibility that considering OpenShift
Enterprise as a viable option for managing containers in a
mission-critical IT shop will require a full top-to-bottom
adoption of RedHat’s container and virtualization products, if both
types of workloads are aimed to be collocated. How OpenShift’s
adoption of Kubernetes and its intent to be a wrapper for it play out
in the long run of the “PaaS war” is sure to be an interesting one.

## Resources

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.
