---
title: Introduction to container technologies: scheduling and management of services and resources
slug: container-technologies-scheduling-management
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Introduction to container technologies: scheduling and management of services and resources
===========================================================================================

Service/Resource Schedulers and Managers on a cluster are tools that are
aware of the underlying resources available, are capable of placing
tasks across the cluster in
a specified and expected manner, abide by rules and constraints, and can
offer the ability to execute tasks and services.

In the container ecosystem, the necessity of these tools becomes evident
in situations such as the lifecycle of installing and maintaining the
Docker engine as well as its dependencies, not to mention setting up the
requirements needed by your applications, and most importantly servicing
the container cluster orchestrator/management you decide to utilize.

To be clear, service/resource schedulers and managers do just that: they
allocate the resources needed to execute a job, such as the execution of
Docker containers.

However, by themselves, these technologies should not be seen as options to
create PaaS offering or to solely orchestrate a set of containers. These
tools serve a far
more basic functionality in retrospect to the requirements actual
Docker services require such as load balancing, failure recovery,
deployment and scaling that are taken care of by an actual orchestrator
sitting on top of your stack. Therefore, just because they can run any
service or task from a simple hello world application to a much more
complex stack across a cluster, to even instantiating a Docker container
on said cluster, this does not mean that they should be in charge of
full orchestration of containers.

The technologies described below are the current front-runners in the
industry with regards to service/resource scheduling.

CoreOS’ “Fleet”
---------------

Fleet is a distributed init system based on etcd for its manifest of
tasks and systemd to do the task execution. It can be seen as an extension
of systemd that operates at the cluster level and can be used to deploy
a systemd unit file anywhere on the cluster.

Fleet can automatically reschedule units on machine failure, and can abide
by properties such as ensuring that units are deployed together on the
same machine, forbid colocation if necessary and deploy to specific
machines based on metadata and attributes.

Apache’s “Mesos”
----------------

Mesos is a distributed systems kernel. It is built using the same
principles as the Linux kernel, only at a different level of abstraction.
The Mesos kernel runs on every machine and provides applications (e.g.,
Hadoop, Spark, Kafka, Elastic Search) with APIs for resource management
and scheduling across entire datacenter and cloud environments (9).

Mesos is a cluster manager that provides efficient isolation of
resources and is truly all about facilitating different types of
workloads (a.k.a frameworks) to run top of it.

Some of the biggest technology companies such as HubSpot and Twitter
are active users and advocates of Mesos.

Comparison
----------
| Org    | Tool  | Req. Supplied Membership | Basic Task Orchestration | Advanced Task Orchestration | Up to  Hundreds of Hosts | Up to  Thousands of Hosts | Language |
|--------|-------|--------------------------|--------------------------|-----------------------------|--------------------------|---------------------------|----------|
| CoreOS | Fleet |             ✓            |             ✓            |                             |             ✓            |                           | Go       |
| Apache | Mesos |             ✓            |             ✓            |              ✓              |                          |             ✓             | C++      |

**Table 2 -­‐ Service/Resource Scheduling & Management Comparison**

| Org    | Tool  | Architecture | Resource Aware | Host Constraints | Host Balancing | Group Affinity | Anti- Affinity | Global Scheduling |
|--------|-------|--------------|----------------|------------------|----------------|----------------|----------------|-------------------|
| CoreOS | Fleet | Monolithic   |                |         ✓        |                |        ✓       |        ✓       |         ✓         |
| Apache | Mesos | Two-level    |        ✓       |         ✓        |        ✓       |                |        ✓       |                   |

**Table 3 – Service / Resource Scheduling and Management Functionality Comparison (10)**

In terms of which technology to use:

-   Fleet is new to the scene and has a decent community following, but
    it seems limited in its capabilities with regard to advance
    scheduling an health metrics. It’s also early in its development,
    and this could possibly accelerate with time.

-   Mesos is the front-runner with some heavy names utilizing it today
    in their infrastructure. Also, Mesosphere, the company that is
    commercializing Mesos and is separate entity from Apache (the
    developer of Mesos) has currently started work on a Mesos
    framework to support Kubernetes and has gotten a good amount of
    traction.

**Current Recommendation** Mesos
