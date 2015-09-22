---
title: Introduction to container technologies: orchestration and management of container clusters
slug: container-technologies-orchestration-clusters
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Introduction to container technologies: orchestration and management of container clusters

Orchestrating and managing a cluster of Docker containers is an emerging
trend that is not only very competitive with many companies, but one
that is evolving at a rapid pace. Many options currently exist with
various feature sets, and the race to be the front-runner is in full
swing.

Below is a list of the notable open source container orchestration engines
and managers, along with a summary of what they each aim to achieve.

It is highly recommended that your respective teams perform the proper
analysis of which option to choose give the use case you intend to
fulfill, as well as the scale yo wish to operate.

## Docker’s “Compose”

Compose, previously known as “Fig” prior to its acquisition, is a simple
orchestration framework really aimed to allow the definition of fast,
isolated development environment for Docker containers.

Its sweet spot really lies in apps that revolve around a single-purpose
server that could easily scale out based on the notion that
architectural complexity is not a requirement. Development environments,
which tend to be an all-in-one baked in means of operation, obviously
fits well into this requirement and thus makes Fig shine as a viable
option.

Based on use cases, something as simple as Fig may be all one needs.
However, because this is a space where a solution such as Fig has both
limited capabilities and overhead, teams may and have decided to hand
roll micro-solutions of this kind on their own for the sake of not taking
on extra overhead in their stack.

Its reception in the community is notable, but the practicality of its
usage and the lack of ability to create a long-term vision around it
tend to minimize the actual legitimacy of adopting it as a container
orchestration technology.

## Prime Directive’s “Flynn”

Prime Directive labels Flynn as “the product that ops provides to
developers (11).” They believe that “ops should be a product team, not
consultants” and that “Flynn is the single platform that ops can provide
to developers to power production, testing, and development, freeing
developers to focus.”

Flynn is an open-source PaaS built from pluggable components that you
can mix and match however you want. Out of the box, it looks a lot like
a Heroku that you could self-host, but one that easily allows you to
replace the pieces you want with whatever you need.

In regards to how Flynn differs from other PaaS like Heroku, Cloud
Foundry, Dei or Dokku, “the other PaaS technologies mainly focus on
scaling a stateless app tier.

They may run one or two persistent services for you, but for the most
part you are on your own to figure that part out. Flynn is really trying
to solve the state problems, which is pretty unique. (12)”

It is worth mentioning that with regards to stateful management,
particularly in databases, right now they support Postgres, but their goal
is to have Flynn help you run the database service so you don’t have to.

Sponsors and users of Flynn include but are not limited to: Coinbase,
Shopify, and CenturyLink.

## OpDemand’s “Deis”

Deis is an open-source PaaS that facilitates the deployment and
management of apps. It is built on Docker and CoreOS (including etcd,
fleet and the OS itself) to “provide lightweight PaaS with
Heroku-­‐inspired workflow (13).”

It can deploy an app or service that works in a Docker container and its
structure mimics Heroku’s 12-factor stateless methodology for how apps
should be created and managed. Deis also leverages Heroku’s Buildpacks
and comes with out-of-the-box support for Ruby, Python, Node.js,
Java, Clojure, Scala, Play, PHP, Perl, Dart and Go.

Much like Flynn, it too looks a lot like a Heroku clone that you could
self-host. However, Deis lacks persistent storage and state aware
support for use cases such as databases, and rather depends on some 3rd
party cloud database solution. In this regard, Flynn seems to be ahead of
Deis as the front-runner in Heroku-like projects.

Users of Deis include small to medium businesses and tech companies, but
no major companies have announced their use of it.

## ClusterHQ’s “Flocker”

Flocker is an open-source data volume and multi-host container
manager that supports and works with Docker’s Compose (a.k.a Fig) file
format syntax. Where Docker naturally shines with applications such as
frontend or API servers, which utilize shared storage and are replicated
or made highly available in some capacity, Flocker’s intention is to
offer the same portability but for applications with systems
such as databases and messaging/queuing systems as state management in
containers is still an incomplete feature that is missing in the
community.

The sweet spot with Flocker seems to be centered on its data management
features and self-proclaiming themselves as the leader in this area.
However, though appearing as the front-runner in datastore-centric
models, full support of data in many use cases is still a work in progress
and operations aren’t met without undergoing downtime of some capacity.

Flocker alleviates the issue of managing data for containers by
utilizing ZFS as the underlying technology for the attached datastore
containers used, with volume behaviors and such operated by ZFS itself.
In addition to the ZFS properties, Flocker imposes a network proxy across
all of the Flocker nodes to handle container linking, storage mapping
and user interaction throughout the cluster.

## Cloudsoft’s “Clocker”

Clocker is an open source project that enables users to spin up Docker
containers, without generating excess containers, in a cloud-agnostic
manner. The project is built on top of Apache Brooklyn, a multi-cloud
application, and management software.

Some features of Clocker are:

-   Automatically create and manage multiple Docker hosts in cloud
    infrastructure

-   Intelligent container placement, providing fault tolerance, easy
    scaling and better utilization of resources

-   Use of any public or private cloud as the underlying infrastructure for
    Docker Hosts

-   Deployment of existing Brooklyn/CAMP blueprints to Docker locations,
    without modifications

Brooklyn uses Apache jclouds, a cloud API agnostic library, to
provision and configure secure communications (SSH) with cloud virtual
machines. The Docker architecture provides ‘containers’ on ‘host’
machines. Brooklyn provisions cloud machines using jclouds and uses
them as Docker hosts.

Brooklyn uses Dockerfile which makes an SSH server available in each
Docker container, after which it can be treated like any virtual
machine. Brooklyn receives sensor data from the app, every docker
host, every docker container as well as every software making up the
app and can effect changes in each of these; enabling Brooklyn to
manage distribution of the app across the Docker Cloud. (14)

In short, Brooklyn is a platform that monitors and manages Docker
containers using a YAML configuration known as blueprints for its
instructions. Clocker then is essentially a blueprint for Brooklyn with
extra intelligence geared at configuring and managing Docker hosts and
containers.

## Mesosphere’s “Marathon”

Marathon is a cluster-­‐wide init and control system for services in
cgroups or Docker containers. It requires and is based on Apache Mesos
and the Chronos job scheduler framework. Where Mesos operates as the
kernel for your datacenter, Marathon is aimed to be the cluster’s init
or upstart daemon. It has a UI and a REST API for managing and
scheduling Mesos frameworks, including Docker containers.

Marathon is a *meta framework*: you can start other Mesos frameworks
with it. It can launch anything that can be launched in a standard shell. In
fact, you can even start other Marathon instances via Marathon (15).
Because it is a framework built on Mesos, it can be seen as a comparable
model to Clocker which is itself a blueprint (analogously a framework)
for Apache’s Brooklyn.

Because of its flexibility, Marathon ca be seen as a cluster-­‐wide
process supervisor, including operating as a private PaaS through
functionality that includes service discovery, failure handling, as well
as deployment and scalability.

As of version 0.19 (current version is at 0.8) containers have become
first class citizens in Marathon, utilizing Deimos as the Docker
containerizer, a.k.a a Docker plugin for Mesos. Using Deimos and being
based on Mesos, the combination of the frameworks allows Marathon to
become an orchestration and management layer for Docker containers and
provides the key services and dependencies one would come to expect in
these particular toolsets.

Many major companies are using Marathon including: Airbnb, eBay,
Groupon, OpenTable, Paypal an Yelp.

## Google’s “Kubernetes”

Kubernetes is a system for managing containerized clusters applications
across multiple hosts, providing basic mechanisms for deployment,
maintenance, and scaling of applications.

Specifically Kubernetes:

-   Uses Docker to package, instantiate, and run containerized
    applications.

-   Establishes robust declarative primitives for maintaining the
    desired state requested by the user. Self-healing mechanisms,
    such as auto-restarting, re-scheduling,
    and replicating containers require active controllers, not
    just imperative orchestration.

-   Is primarily targeted at applications comprised of multiple
    containers, such as elastic, distributed micro-services.

-   Enables users to ask a cluster to run a set of containers.
    The system automatically chooses hosts to run those containers on
    using a scheduler that is policy-rich, topology-aware and workload-specific.

Kubernetes builds upon a decade and a half of experience at Google running
production workloads at scale, combined with best-of-breed ideas and
practices from the community. It is written in Golang and is lightweight,
modular, portable and extensible (16).

Some of the concepts behind Kubernetes include:

-   **Pods:** a way to collocate group containers together with shared
    volumes, [or even more explicitly: a collocation of 1 or more
    containers that share a single IP address, multiple volumes and a
    single set of ports].

-   **Replication Controllers:** a way to handle the lifecycle of pods. They
    ensure that a specified number of pods are running at any given
    time, by creating or killing pods as required.

-   **Labels:** a way to organize and select groups of objects based o
    key/value pair.

-   **Services:** a set of containers performing a common function with
    single, stable name and address for a set of pods – They act like a
    basic load balancer (17).

Being one of the hottest, if not *the* hottest, technologies in the
Docker ecosystem right now has forced many folks in the community to
compare it to Mesos, the leader in cluster oriented development and
management for the past couple of years.

However, the two have their differences:

With Mesos, there is a fair amount of overlap in terms of the basic
vision, but the products are at quite different points in their
lifecycle and have different sweet spots. Mesos is a distributed
systems kernel that stitches together a lot of different machines into
a logical computer. It was born for a world where you own lot of
physical resources to create a big static computing cluster.

The great thing about it is that lots of modern scalable data processing
applications run well on Mesos (Hadoop, Kafka, Spark) and it is nice
because you can run them all on the same basic resource pool, along
with your new age container packaged apps. It is somewhat more heavy
weight than the
Kubernetes project, but is getting easier and easier to manage thanks
to the work of folks like Mesosphere.

Now what gets really interesting is that Mesos is currently being
adapted to add a lot of the Kubernetes concepts and to support the
Kubernetes API. So it will be a gateway to getting more capabilities
for your Kubernetes app (high availability master, more advanced
scheduling semantics, ability to scale to a very large number of
nodes) if you need them, and is well suited to run production
workloads.

Lastly, [some say] Kubernetes and Mesos [can be] a match made in heaven.
Kubernetes enables the Pod, along with Labels for service discovery,
load-­‐balancing, and replication control. Mesos provides the
fine-­‐grained resource allocations for pods across nodes in a cluster,
and facilitates resource sharing among Kubernetes and other frameworks
running on the same cluster (18). However, Mesos can easily be replaced by
OpenStack and if you’ve bought into Openstack, then the dependency and
usage of Meso can be elimated. To get back to the comparison,
Kubernetes is an opinionated declarative model on how to address
microservices, and Mesos is the layer that provides an imperative
framework by which developers can define a scheduling policy in a
programmatic fashion – when leveraged together, they provide a
datacenter with the ability to do both.

The main take-away for Kubernetes is that right now it is best fit for
typical webapps and stateless applications and that it’s in
pre-production beta. However, Kubernetes is one of the most active and
tracked projects on Github, so expect many changes in not only its
functionality, stability and supported use cases, but also in the amount
of technologies working on becoming highly interoperable with Kubernetes.

## Docker’s “Swarm”

Swarm is a tier aimed to provide a common interface onto the many
orchestration and scheduling frameworks available. It serves as
clustering and scheduling tool that optimizes the infrastructure based
on requirements of the app and performance. Solomon Hykes, CTO of
Docker, stated, “Docker will give devs a standard interface to all
[orchestration tools] and [Swarm] is an ingredient of that standard
interface. [I can be thought of as the glue between Docker and orchestration
backends.”

Swarm is designed to provide a smooth Docker deployment workflow,
working with some existing container workflow frameworks such as Deis,
but flexible enough to yield to "heavyweight" deployment and resource
management such as Mesos. It is said to be a very simple add-­‐on to
Docker. It currently does not provide all the
features to say something of the likes of Kubernetes and its usage and
place in the ecosystem is still to be determined.

## Comparison and recommendations

| Org             | Tool       | One  Host (nano) | Up to  Tens of Hosts (micro) | Up to  Hundreds of Hosts (medium) | Up to  Thousands of Hosts (large) |
|-----------------|------------|------------------|------------------------------|-----------------------------------|-----------------------------------|
| Docker          | Compose    |         ✓        |                              |                                   |                                   |
| Prime Directive | Flynn      |                  |               ✓              |                                   |                                   |
| OpDemand        | Deis       |                  |               ✓              |                                   |                                   |
| ClusterHQ       | Flocker    |                  |               ✓              |                                   |                                   |
| CloudSoft       | Clocker    |                  |                              |                 ✓                 |                                   |
| Mesosphere      | Marathon   |                  |                              |                                   |                 ✓                 |
| Google          | Kubernetes |                  |                              |                                   |                 ✓                 |

**Table 4 -‐ Size Comparison of Container Orchestrators & Managers**

| Org             | Tool       | Cluster State Management | Monitoring &  Healing | Deploy Spec                                 | Allows Docker Dependency & Architectural Mapping | Deployment Method | Language |
|-----------------|------------|--------------------------|-----------------------|---------------------------------------------|--------------------------------------------------|-------------------|----------|
| Docker          | Compose    |                          |                       | Dockerfile + YAML manifest                  |                         ✓                        | CLI               | Python   |
| Prime Directive | Flynn      |                          |                       | Procfile,  Heroku Buildpack                 |                                                  | git push          | Go       |
| OpDemand        | Deis       |                          |                       | Dockerfile,  Heroku Buildpack               |                                                  | git push          | Go       |
| ClusterHQ       | Flocker    |             ✓            |                       | Dockerfile + YAML manifest                  |                         ✓                        | CLI               | Python   |
| CloudSoft       | Clocker    |             ✓            |           ✓           | Apache Brooklyn YAML blueprint + Dockerfile |                         ✓                        | API / Web         | Java     |
| Mesosphere      | Marathon   |             ✓            |           ✓           | JSON                                        |                         ✓                        | API / CLI         | C++      |
| Google          | Kubernetes |             ✓            |           ✓           | YAML / JSON                                 |                         ✓                        | API / CLI         |          |


**Table 5 -­‐ Functionality Comparison of Container Orchestrators and
Managers**

![Strata of the container ecosystem](images/container-ecosystem-layers.jpg)

**Figure 1 -­‐ Strata of Container Ecosystem (19)**
(Note: OpenStack can also be options at layers 2 & 5)


![Deploy and manage code as app in PaaS](images/container-paas-code-as-app.jpg)

**Figure 2 -­‐ Venn Diagram of Container Orchestrator and Managers**

**Current Recommendation:** Kubernetes

XXXXXXXX
2 images fit in this section somewhere
Nate is rebuilding them from https://github.com/rose-coste/whitepapers/tree/master/images
XXXXXXXXX

## Resources

For detailed background information about the ecosystem of container technologies
for the Rackspace Container Service,
follow the links suggested at
[Introduction to container ecosystems and technologies: suggested reading]
(/container-technologies-references/).
