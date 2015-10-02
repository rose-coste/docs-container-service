---
title: Introduction to container technologies: orchestration and management of container clusters
permalink: docs/best-practices/container-technologies-orchestration-clusters/
description: Introduction to container technologies, powered by the Rackspace Container Service
author: Mike Metral
date: 2015-10-01
topics:
  - best-practices
  - planning
---

*The best tool for orchestration and management of container clusters varies with the size of the cluster: Kubernetes and Marathon excel with thousands of hosts while Compose is ideal for a single host.*

Orchestrating and managing a cluster of Docker containers is an emerging
trend that is not only very competitive with many companies, but one
that is evolving at a rapid pace. Many options currently exist with
various feature sets, and the race to be the front-runner is in full
swing.

Below is a discussion of notable open-source container orchestration engines and managers, along with a summary of what they each aim to achieve. This is a general introduction to those tools; before you adopt any of them, you should perform your own careful analysis of which option to choose given the use case you intend to fulfill and the scale at you wish to operate.

## Docker’s “Compose”

Compose, known as “Fig” prior to its acquisition by Docker, Inc, is a simple
orchestration framework intended to allow the definition of fast,
isolated development environments for Docker containers.

Its sweet spot really lies in applications that revolve around a single-purpose
server that could easily scale out based on the notion that
architectural complexity is not a requirement. Development environments,
which tend to bake in an all-in-one method of operation, obviously
fit well into this requirement. This makes Compose shine as a viable
option.

Based on use cases, something as simple as Compose may be all that one needs. However, because this is a space in which a solution such as Compose has both limited capabilities and overhead, teams can decide to independently develop micro-solutions of this kind for the sake of not taking on extra overhead in their stack.

The community's reception of Compose has been notably positive, but the practicality of its usage and the lack of ability to create a long-term vision around it tend to minimize the actual legitimacy of adopting it as a container orchestration technology.

## Prime Directive’s “Flynn”

Prime Directive labels Flynn as “the product that ops provides to
developers [(1)](#resources).” They believe that “ops should be a product team, not
consultants” and that “Flynn is the single platform that ops can provide
to developers to power production, testing, and development, freeing
developers to focus.”

Flynn is an open-source Platform-as-a-Service built from pluggable components that you
can mix and match however you want. Out of the box, it looks a lot like
a Heroku that you could self-host, but one that easily allows you to
replace the pieces you choose with whatever you need.

In regards to how Flynn differs from other PaaS like Heroku, Cloud
Foundry, Deis, or Dokku, “the other PaaS technologies mainly focus on
scaling a stateless app tier. They may run one or two persistent services for you, but for the most
part you are on your own to figure that part out. Flynn is really trying
to solve the state problems, which is pretty unique [(2)](#resources).”

It is worth mentioning that with regards to stateful management,
particularly in databases, right now Flynn supports Postgres, but their goal
is to have Flynn help you run the database service so you don’t have to.
To learn more about working with data and stateful applications in containers, read [Docker best practices: data and stateful applications](/docker-best-practices-data-stateful-applications/).

Sponsors and users of Flynn include but are not limited to Coinbase,
Shopify, and CenturyLink.

## OpDemand’s “Deis”

Deis is an open-source PaaS that facilitates the deployment and
management of applications. It is built on Docker and CoreOS (including etcd,
fleet and the operating system itself) to “provide lightweight PaaS with
Heroku-­inspired workflow [(3)](#resources).”
To learn more about the need for container-focused operating systems such as CoreOS, read [Introduction to container technologies: container operating systems](/container-technologies-operating-systems/).

Deis can deploy an application or service that works in a Docker container and its
structure mimics Heroku’s 12-factor stateless methodology for how apps
should be created and managed. Deis also leverages Heroku’s Buildpacks
and comes with out-of-the-box support for Ruby, Python, Node.js,
Java, Clojure, Scala, Play, PHP, Perl, Dart and Go.

Much like Flynn, Deis looks a lot like a Heroku clone that you can
self-host. However, Deis lacks persistent storage and state-aware
support for use cases such as databases; instead, Deis depends on a third-party
cloud database solution. In this regard, Flynn seems to be ahead of
Deis as the front-runner in Heroku-like projects.

Users of Deis include small to medium businesses and technology companies, but
no major companies have announced their use of it.

## ClusterHQ’s “Flocker”

Flocker is an open-source data volume and multi-host container manager that supports and works with the file format syntax used by Docker’s Compose (formerly known as Fig). Where Docker naturally shines with applications such as
frontend or API servers, which utilize shared storage and are replicated
or made highly available in some capacity, Flocker’s intention is to
offer the same portability but for applications with systems
such as databases and messaging/queuing systems. State management in
containers is still an incomplete feature that is missing in the
community, giving Flocker an opportunity to meet this need.

The sweet spot with Flocker seems to be centered on its data management
features; they have proclaimed themselves as the leader in this area.
However, though appearing as the front-runner in datastore-centric
models, full support of data in many use cases is still a work in progress
and operations aren not met without undergoing downtime of some capacity.

Flocker alleviates the issue of managing data for containers by
utilizing the Z File System (ZFS) as the underlying technology for containers' attached datastore, with volume behaviors and such operated by ZFS itself.
In addition to the ZFS properties, Flocker imposes a network proxy across
all of the Flocker nodes to handle container linking, storage mapping
and user interaction throughout the cluster.

## Cloudsoft’s “Clocker”

Clocker is an open source project that enables users to spin up Docker
containers in a cloud-agnostic manner without generating excess containers. The project is built on top of Apache Brooklyn, undergoing incubation at the Apache Software Foundation as a tool for modeling, deploying, and managing multi-cloud application software.

Some features of Clocker are:

- Automatic creation and management of multiple Docker hosts in cloud
  infrastructure

- Intelligent container placement, providing fault tolerance, easy
  scaling, and efficient utilization of resources

- Use of any public or private cloud as the underlying infrastructure for
  Docker Hosts

- Deployment of existing Brooklyn/CAMP blueprints to Docker locations,
  without modifications

Brooklyn uses Apache jclouds, a multi-cloud toolkit, to
provision and configure secure communications (SSH) with cloud virtual
machines. The Docker architecture provides containers on host
machines. Brooklyn provisions cloud machines using jclouds and uses
them as Docker hosts.

Brooklyn uses Dockerfile which makes an SSH server available in each
Docker container, after which the container can be treated like any virtual
machine. Brooklyn receives sensor data from the application, every Docker
host, every Docker container, and every software component making up the
application and can effect changes in each of these. This enables Brooklyn to
manage distribution of the application across the Docker Cloud [(4)](#resources).

In short, Brooklyn is a platform that monitors and manages Docker
containers using a YAML configuration known as blueprints for its
instructions. Clocker then is essentially a blueprint for Brooklyn with
extra intelligence geared at configuring and managing Docker hosts and
containers.

## Mesosphere’s “Marathon”

Marathon is a cluster-­‐wide init and control system for services in
cgroups (Linux kernel control groups) or Docker containers. It requires and is based on Apache Mesos
and the Mesosphere Chronos job scheduler framework. Where Mesos operates as the
kernel for your datacenter, Marathon is aimed to be the cluster’s init
or upstart daemon. Marathon has a UI and a REST API for managing and
scheduling Mesos frameworks, including Docker containers.

Marathon is a *meta framework*: you can start other Mesos frameworks
with it. It can launch anything that can be launched in a standard shell. In
fact, you can even start other Marathon instances via Marathon [(5)](#resources).
Because Marathon is a framework built on Mesos, it can be seen as a comparable
model to Clocker which is itself a blueprint (analogous to a framework)
for Apache’s Brooklyn.

Because of its flexibility, Marathon can be seen as a cluster-­wide
process supervisor. Marathon operates as a private Platform-as-a-Servuce through
functionality that includes service discovery, failure handling, deployment, and scalability.

With Marathon, containers are first-class citizens, utilizing Deimos as if it were a Docker containerizer plugin for Mesos. Using Deimos while being
based on Mesos, this combination of frameworks allows Marathon to
become an orchestration and management layer for Docker containers and
provides the key services and dependencies one would expect in
these particular toolsets.

Many major companies are using Marathon including Airbnb, eBay,
Groupon, OpenTable, Paypal, and Yelp.

## Google’s “Kubernetes”

Kubernetes is a system for managing containerized clusters applications
across multiple hosts. It provides basic mechanisms for deployment,
maintenance, and scaling of applications.

Specifically, Kubernetes:

- Uses Docker to package, instantiate, and run containerized
  applications.

- Establishes robust declarative primitives for maintaining the
  desired state requested by the user. Because it has active controllers, not
  just imperative orchestration, it enables self-healing mechanisms
  such as auto-restarting, re-scheduling,
  and replicating containers.

- Is primarily targeted at applications comprised of multiple
  containers, such as elastic, distributed micro-services.

- Enables users to ask a cluster to run a set of containers.
  The system automatically chooses hosts on which to run those containers,
  using a scheduler that is policy-rich, topology-aware, and workload-specific.

Kubernetes builds upon a decade and a half of experience at Google running
production workloads at scale, combined with best-of-breed ideas and
practices from the community. It is written in Golang and is lightweight,
modular, portable and extensible [(6)](#resources).

### Kubernetes concepts

Some of the key ideas behind Kubernetes include:

- **pods:** A way to co-locate group containers with shared
  volumes. A pod is a collocation of one or more
  containers sharing a single IP address, multiple volumes, and a
  single set of ports.

- **replication controllers:** A way to handle the lifecycle of pods. They
  ensure that a specified number of pods are running at any given
  time, by creating or killing pods as required.

- **labels:** A way to organize and select groups of objects based on
  key/value pair.

- **services:** A set of containers performing a common function with a
  single, stable name and address for a set of pods. Services act like a
  basic load balancer [(7)](#resources).

### Comparing Kubernetes and Mesos
Being one of the hottest, if not *the* hottest, technologies in the
Docker ecosystem right now has forced many comparisons of Kubernetes to
Mesos, the leader in cluster-oriented development and
management for the past couple of years.

However, while there is some overlap in terms of their basic vision, Kubernetes and Mesos have important differences. The products are at quite different points in their
lifecycles and have different sweet spots. Mesos is a distributed
systems kernel that stitches together many different machines into
a logical computer. It was born for a world in which you own many
physical resources anc can combine them to create a big static computing cluster.

Many modern scalable data processing
applications (Hadoop, Kafka, Spark) run well on Mesos and you can run them all on the same basic resource pool, along
with modern container-packaged applications. Mesos is somewhat more heavyweight than the
Kubernetes project, but is getting easier and easier to manage thanks
to the work of folks like Mesosphere.

Now what gets really interesting is that Mesos is currently being
adapted to add a lot of the Kubernetes concepts and to support the
Kubernetes API. So Mesos will be a gateway to getting more capabilities
for your Kubernetes application, such as a high-availability master, more advanced
scheduling semantics, and the ability to scale to a very large number of
nodes. This will make Mesos well suited to run production
workloads.

Lastly, some say Kubernetes and Mesos can be a match made in heaven:

- Kubernetes enables the pod, along with labels for service discovery,
  load-balancing, and replication control.
- Mesos provides the fine-grained resource allocations for pods across nodes in a cluster,
  and facilitates resource sharing among Kubernetes and other frameworks
  running on the same cluster [(8)](#resources).

However, Mesos can easily be replaced by
OpenStack and if you’ve bought into Openstack, then the dependency on and
usage of Mesos can be eliminated.

To get back to the comparison,
Kubernetes is an opinionated declarative model of how to address
microservices, and Mesos is the layer that provides an imperative
framework by which developers can define a scheduling policy in a
programmatic fashion. When leveraged together, they provide a
datacenter with the ability to do both.

### Best fits for Kubernetes

The main take-away for Kubernetes is that right now it is best fit for
typical webapps and stateless applications and that it is in
pre-production beta. However, Kubernetes is one of the most active and
tracked projects on Github, so expect many changes in not only its
functionality, stability and supported use cases, but also in the number
of technologies working to become highly interoperable with Kubernetes.

## Docker’s “Swarm”

Swarm is a tier aimed to provide a common interface onto the many
orchestration and scheduling frameworks available. It serves as a
clustering and scheduling tool that optimizes the infrastructure based
on requirements of the application and performance. Solomon Hykes, CTO of
Docker, stated, “Docker will give devs a standard interface to all
[orchestration tools] and [Swarm] is an ingredient of that standard
interface. [It] can be thought of as the glue between Docker and orchestration
backends [(9)](#resources).”

Swarm is designed to provide a smooth Docker deployment workflow,
working with some existing container workflow frameworks such as Deis,
but flexible enough to yield to heavyweight deployment and resource
management such as Mesos. It is said to be a very simple add-on to
Docker. It currently does not provide all the
features of something as comprehensive as Kubernetes and its usage and
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

**Table 4 -‐ Size comparison of container orchestrators and managers**

| Org             | Tool       | Cluster State Management | Monitoring &  Healing | Deploy Spec                                 | Allows Docker Dependency & Architectural Mapping | Deployment Method | Language |
|-----------------|------------|--------------------------|-----------------------|---------------------------------------------|--------------------------------------------------|-------------------|----------|
| Docker          | Compose    |                          |                       | Dockerfile + YAML manifest                  |                         ✓                        | CLI               | Python   |
| Prime Directive | Flynn      |                          |                       | Procfile,  Heroku Buildpack                 |                                                  | git push          | Go       |
| OpDemand        | Deis       |                          |                       | Dockerfile,  Heroku Buildpack               |                                                  | git push          | Go       |
| ClusterHQ       | Flocker    |             ✓            |                       | Dockerfile + YAML manifest                  |                         ✓                        | CLI               | Python   |
| CloudSoft       | Clocker    |             ✓            |           ✓           | Apache Brooklyn YAML blueprint + Dockerfile |                         ✓                        | API / Web         | Java     |
| Mesosphere      | Marathon   |             ✓            |           ✓           | JSON                                        |                         ✓                        | API / CLI         | C++      |
| Google          | Kubernetes |             ✓            |           ✓           | YAML / JSON                                 |                         ✓                        | API / CLI         |          |

**Table 5 -­‐ Functionality comparison of container orchestrators and
managers**

XXXXXXXX
2 images fit in this section somewhere
Nate is rebuilding them from https://github.com/rose-coste/whitepapers/tree/master/images
XXXXXXXXX

![Strata of the container ecosystem](images/container-ecosystem-layers.jpg)

**Figure 1 -­‐ Strata of container ecosystem  [(10)](#resources)**

(Note: OpenStack can also be options at layers 2 & 5)

![Deploy and manage code as app in PaaS](images/container-paas-code-as-app.jpg)

**Figure 2 -­‐ Venn diagram of container orchestrators and managers**

**Current Recommendation:** Kubernetes

XXXXXXXX
2 images fit in this section somewhere
Nate is rebuilding them from https://github.com/rose-coste/whitepapers/tree/master/images
XXXXXXXXX

<a name="resources"></a>
## Resources

*Numbered citations in this article*

1. <https://flynn.io/>

2. <http://www.centurylinklabs.com/interviews/what-is-flynn-an-open-source-docker-paas/>

3. <http://deis.io/overview/>

4. <http://www.infoq.com/news/2014/06/clocker>

5. <https://github.com/mesosphere/marathon>

6. <https://github.com/GoogleCloudPlatform/kubernetes>

7. <http://stackoverflow.com/questions/26705201/whats-the-difference-between-apaches-mesos-and-googles-kubernetes>

8. <https://github.com/mesosphere/kubernetes-mesos/blob/master/README.md>

9. <https://twitter.com/solomonstre/status/492111054839615488>

10. <https://pbs.twimg.com/media/B33GFtNCUAE-vEX.png:large>

*Other recommended reading*

- [Docker best practices: data and stateful applications](/docker-best-practices-data-stateful-applications/)

- [Introduction to container technologies: container operating systems](/container-technologies-operating-systems/).

- <http://12factor.net/>

- <https://elements.heroku.com/buildpacks>

- <http://open-zfs.org/wiki/Main_Page>

- <https://brooklyn.incubator.apache.org/>

- <https://jclouds.apache.org/>

- <http://yaml.org/>

In addition to *best-practices* articles such as this one,
Rackspace Container Service documentation includes *tutorials* and *references*:

* For step-by-step demonstrations, explore the *tutorials* collection.
* For detailed descriptions of reference architectures designed
  for specific use cases,
  explore the *references* collection.
* For discussions of key ideas, recommendations of useful methods and tools, and
  general good advice, explore the *best-practices* collection.

## About the author

Mike Metral is a Product Architect at Rackspace. You can follow him in GitHub at https://github.com/metral and at http://www.metralpolis.com/.
