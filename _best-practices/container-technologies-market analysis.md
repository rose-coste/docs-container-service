---
title: Introduction to container technologies: market analysis
slug: container-technologies-market-analysis
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Introduction to container technologies: market analysis
=======================================================

This report is intended to cover and educate on the various
technologies emerging in and around the container space without deep
diving too far into the intrinsic concepts and specifics of any one given
technology. Specifically, this report should be seen as a means to get
up to speed on what containers are as well as the architecture
decisions that exist in adapting containers into both your
infrastructure and toolbox. In addition, this report will highlight what container systems
and utilities serve as the best solution depending on the use case, as
well as which of these options compete and overlap with one another.

The container ecosystem is one of the fastest evolving trends in tech
space that we’ve seen since the virtualized movement of the previous
decade. As such, it should be made known that the findings and
opinions expressed in this report are *extremely* time sensitive as a
small timespan of merely 6 months can and has rapidly shifted what
the community has come to think and consider as a viable option.

Lastly, a lot of the information detailed in this report has been a
result of personal experience using the technologies, but this is not
meant to take away from the influence of various community
discussions, articles and blog posts, and accrediting the exact source
has proven to not be fully possible because of the author’s lack of not
noting the source.

Overview
--------

Th hype and obsession of containers in the last two years becomes evident
when you think of IT infrastructure from a traditional virtualization
perspective and the implication that one can be utilizing their
computing resources with an even further elasticity and capability.

In short, the virtualization we’ve all come to know and use today is
possible because of the development and rise of the hypervisor.

So why does everyone love containers and Docker? James Bottomley,
Parallels' CTO of server virtualization and leading Linux kernel
developer, explained to me that VM hypervisors, such as Hyper-V,
KVM, and Xen, all are "based on emulating virtual hardware. That means
they’re fat in terms of system requirements."

Containers, however, use shared operating systems. That means they are
much more efficient than hypervisors in system resource terms. Instead
of virtualizing hardware, containers rest on top of a single Linux
instance [on th host]. This in turn means you can “leave behind
the useless 99.9% VM junk, leaving you with a small, neat capsule
containing your application,” said Bottomley.

Therefore, according to Bottomley, with a perfectly tuned container
system, you can have as many as four-to-six times the number of
server application instances as you can using Xen or KVM VMs on the same
hardware” (1).

Analysis
--------

The immense evolution that containers are bringing forth has created a
space for a slew of technologies and tools to emerge, particularly at
the orchestration level.

Being somewhat of the Wild West right now in terms of competition does
not mean that one tool is the be-all and end-all to enable a
container offering.

Viewing the ecosystem as a complimentary vertical stack rather than a
horizontal one where they are constantly bumping heads helps alleviate
the confusion between choosing one tool over the other, as many tools
can and will be interoperable with one another. In doing so, this
ultimately enables and offers a new stack to power and operate your apps
and services.

That’s not to say that some tools aren’t competing with each other
directly, or stepping into areas that blur the lines. Because this is
such a fast-paced movement, it is expected that the messaging,
functionality and roadmap for all tools in this domain will constantly
fluctuate.

It is strongly recommended that one should closely track, survey and
implement the tools accordingly, based not only on the requirements you
would need for you container offering, but more importantly, the
popularity of the tool and its reception in the strong community that
serves as its driving force.

CONCLUSION FROM PART2: INTEGRATE

Docker has spawned a slew of technical opportunities to revamp and
reconstitute what datacenters and application stacks should look and
operate like. There are varying degrees of features and capabilities
that span and even spill over from one technology to the other, and
noticing these subtleties is a very complex and tedious task. Figuring
out which set of technologies will help you establish your future
roadmap comes down to the types of users you’re trying to enable, as
well as classes of workloads you wish to manage across your resources.

It is the author’s ambition that this document has provided not just
beneficial information with regards to using Docker correctly and
efficiently, but that a light has been shined on how these varying
tools function, how they're being received in the community, and how
they compare with one another.
