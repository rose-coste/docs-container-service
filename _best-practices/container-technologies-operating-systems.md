Introduction to container technologies: container operating systems
===================================================================

With the success and popularity of containers made by Docker, modern
operating systems have emerged embracing the container culture. These
OS’ tend to provide the minimal functionality required to deploy
applications along with self-updating and healing properties which
are different than standard OS’s today. In doing so, these types of OS
have evolved the operating model users are accustomed to by moving
away from deploying applications at the application layer to deploying
applications inside containers operated by Docker. More simply put,
applications can be thought of as self-contained binaries that can be
moved around environments accordingly based on your requirements of
QOS, policy, affinity, replication etc.

CoreOS
------

CoreOS is a new Linux distribution that has been architected to provide
features needed to run modern infrastructure stacks via containers
with a hands-off
approach to keeping the OS up to date much like the manner in which
browsers receive updates. Th strategies and architectures that influence
CoreOS are similar to the mechanisms that allow companies like Google,
Facebook and Twitter to run their services at scale with high resilience.

In addition to the self-updating nature of CoreOS, the real value lies in
its flagship products:

-   **etcd** highly-available key value store for shared configuration
    and service discovery.

-   **fleet**: distributed init system that uses etcd as its manifest
    and systemd as its mechanism for instantiating units. Units are
    configuration files that describe the properties of the process
    that you'd like to run. One could think of it as an extension of
    systemd that operates at the cluster level instead of the machine
    level, so it functions as a simple orchestration system for systemd
    units across your cluster.

Red Hat Project Atomic
----------------------

Red Hat’s Project Atomic facilitates application-centric IT architecture
by providing a end-to-end solution for deploying containerized
applications quickly and reliably, with atomic update and rollback for
application and host alike.

The core of Project Atomic is the Project Atomic Host. This is a
lightweight operating system that has been assembled out of upstream RPM
content. It is designed to run applications in Docker containers. Hosts
based on Red Hat Enterprise Linux and Fedora are available now. Hosts
based on CentOS will be available soon.

Project Atomic hosts inherit the full features and advantages of their
base distributions. This includes systemd, which provides
container dependency management and fault recovery. It also includes
journald, which provides secure aggregation and attribution of container
logs (6).
