---
title: Introduction to container technologies: registration and discovery of container services
slug: container-technologies-registration-discover
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Introduction to container technologies: registration and discovery of container services

Service registration discovery is the centerpiece in systems that are
both distributed and service oriented. Pinpointing how nodes in a cluster
can register, discover and determine the necessary services available,
and the means by which they can communicate with said service is the
heart of the problem that service discovery technologies aim to solve.

In an active environment, containers are constantly being commissioned
and decommissioned based on needs, standards, maintenance and failures.
As you scale out your container architecture, keeping track of all the
services in a static manner
simply won’t cut it, and a dynamic means of avoiding disturbance or
disruption to your services is required.

The technologies described below are the current front-­‐runners in the
industry with regards to service registration, and in turn, service
discovery.

## Apache’s “Zookeeper”

Zookeeper is a distributed configuration service synchronization service
and naming registry for large distributed systems. ZooKeeper was a
sub-project of Hadoop, but is now a top-level project in its own right.

It holds Consistency+Partition Tolerance (CP) architecture, in the CAP
theorem context, and uses the Zab protocol to coordinate changes across the
cluster.

Many projects use Zookeeper, including: Hadoop’s HBase, Yahoo and
Rackspace’s Email & Apps.

## CoreOS’ “etcd”

Etcd is a distributed key/value store used for service discovery and shared
configuration. It is aimed to be a simple implementation of the Raft
consensus algorithm, particularly, with regards to agreements on
election cycle and leader nomination, as well as manipulation of data.

I holds Consistency+Partition Tolerance (CP) architecture, in the CAP
theorem context, and chooses consistency over availability, specifically
sequential consistency based on a quorum of nodes.

Many projects use etcd, including: Google’s Kubernetes, Pivotal’s Cloud
Foundry, Rackspace’s Mailgun, Apache Mesos & Mesosphere DCOS (7).

## Hashicorp’s “Consul”

Consul is a tool for service discovery and configuration. It is distributed,
highly available, and extremely scalable. Key features include:

-   **Service Discovery:** Consul makes it simple for services to
    register themselves and to discover other services via DNS or HTTP
    interface. External services such as SaaS providers can be
    registered as well.

-   **Health Checking:** Health Checking enables Consul to quickly
    alert operators about any issues in a cluster. The integration with
    service discovery prevents routing traffic to unhealthy hosts and
    enables service level circuit breakers.

-   **Key/Value Storage:** Flexible key/value store enables storing
    dynamic configuration, feature flagging, coordination, leader
    election and more. The simple HTTP API makes it easy to use
    anywhere.

-   **Multi-­‐Datacenter:** Consul is built to be datacenter aware,
    and can support any number of regions without complex
    configuration (9).

Consul holds Consistency+Partition Tolerance (CP) architecture, in the CAP
theorem context, and implements the Raft protocol.

Public projects or companies using Consul were not found in a limited
search performed.

## Comparison and recommendations

| Org       | Tool      | Client/Server  Architecture | Primitive Key/Value Store | Basic  Service Discovery | Advanced Service Discovery | Consistency | Language |
|-----------|-----------|-----------------------------|---------------------------|--------------------------|----------------------------|-------------|----------|
| Apache    | Zookeeper |              ✓              |             ✓             |             ✓            |                            |      ✓      | Java     |
| Hashicorp | Consul    |              ✓              |             ✓             |                          |              ✓             |      ✓      | Go       |
| CoreOS    | Etcd      |              ✓              |             ✓             |             ✓            |                            |      ✓      | Go       |

**Table 1 -­‐ Service Registration & Discovery Comparison**

Note: Basic vs. Advanced Service Discovery revolves around the notion
that in an advanced setting, the technology has more service
monitoring and health-checking capabilities.

In terms of which technology to use:

-   Zookeeper has been around longer and thus is considered to be mature,
    but its dependency and usage of Java tends to deter many users. It
    is also worth noting many think that it may be starting to show some
    age and its adaptability into cloud infrastructures isn’t the
    easiest process as its proven to be complicated, hard to work with
    and troubleshoot.

-   Etcd and Consul are both new to the scene, but etcd is slightly older
    than Consul and the community seems to be favoring as well as
    using etcd far more.

**Current Recommendation** etcd

## Resources

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.
