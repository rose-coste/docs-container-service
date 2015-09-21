---
title: Docker best practices: data and stateful applications
slug: docker-best-practices-data-stateful-applications
description: Docker best practices, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Docker best practices: data and stateful applications
=====================================================

As soon as one gets over the initial hurdle of what containers can
provide, the next natural step is to build an actual stack that could
benefit from Docker. A common use-case is a 3-tier webapp with a
frontend, middle logic layer and backend database. In doing so, the
frontend and middle logic layers are perfect fits to be containerized,
if they are stateless. However, when you get to thinking about
stateful information such as a backend database layer or data in
general that needs to be preserved, used and possibly written to logs,
containers aren’t the best match, yet.

Datastore and logs
------------------

There are strong recommendations that you should never store data or
logs in a container as the longevity of a container is short-­‐lived
and containers themselves are conceptually ephemeral, more so than
VM’s. Instead, one should store the data and logs by leveraging
Docker’s volume mounts to create either a data volume or a data volume
container that is used and shared by other containers.

Using volume mounts creates a specially designed directory within one
or more containers that bypasses the Union File System and provides a
set of features for persistent and shared data:

-   Volumes are initialized when a container is created. If the
    container's base image contains data at the specified mount point,
    that data is copied into the new volume.

-   Data volumes can be shared and reused among containers.

-   Changes to a data volume are made directly.

-   Changes to a data volume will not be included when you update an
    image.

-   Data volumes persist even if the container itself is deleted.

**Data volumes** are designed to persist data, independent of the
container's life cycle. Docker, therefore, [will] *never* automatically
delete volumes when you remove a container, nor will it "garbage
collect" volumes that are no longer referenced by a container (31). Data
volumes are created by either using the \`-­‐v\` flag in \`docker run\`,
or by using the \`VOLUME\` instruction in your Dockerfile – in either
case, both options ultimately do the same task: create a volume in the
container that is mapped to a directory on the host itself, but the crux
being that **its location is irrelevant to us**. The only varying factor
between both options is that with the \`-­‐v\` flag you can specify to
mount a particular file or directory of your choosing from the host, by
issuing a command such as \`docker run –v /host/src/foobar:/opt/foobar\`
if you do care about the location of the volume’s from the host
perspective.

**Data volume containers** operate in a similar fashion to Data volumes,
but are designed to persist data that you want to share between
containers, or want to use from non-­‐persistent containers. To use the
Data Volume container with other containers, you first start by creating
it: \`docker create –v /data –name datastore mysql\`. This command will
start a container and exit immediately, because it is used as a
container shell that references a newly created volume (that other
containers will use), and not an active process like other application
containers. Once the Data volume container exits, you can reference it
from other containers by using the \`-- volumes-from\` flag on the
subsequent containers such as: \`docker run –d --volumes- from datastore
–name mywebapp foobar/webapp\`. If you remove the datastore container or
the containers that reference it, again it wont delete the volume where
your data is stored. To delete the volume from disk, you must explicitly
call \`docker rm –v\` against the last container with a reference to the
volume (32). This allows you to upgrade, or effectively migrate data volumes
between containers or even the containers using the data volume
container with no worries of losing the data itself.

Using volumes with containers is great because when you need to back up
your database, logs etc. living in the volumes, which in turn live on
the host, you would use your specific tools on the host itself as you’ve
been doing in your organization before containers showed up. After doing
so, you can carry out tasks such as upgrading the image itself by
shutting the previous container down and starting the updated one with
the same volume(s), or one can perform any other routine updates and
maintenance. In summary, the general rule of thumb is that if it gets
written to, it should be done in a volume.

Using a Data volume container sounds like a great solution to the data
management issue, in theory, however, what happens when you want to
scale out where your data resides or when your host machine goes down
in say events such as a restart due to maintenance, or possibly
failure? Well the short answer is that there is no great answer. The
current standard is the same one we’ve come to know, and that is to
effectively relay the data management tasks to self-managed systems
such as using dedicated rsyslog servers for logs, or redundant cloud
services such as cloud block storage and cloud database as a service
to persist your database, but these could introduce concerns such as
cloud vendor lock-in and performance implications. Another strategy
is to integrate the data management back into the stateless
applications as we’ve been accustomed to, but doing so only creates
the monolithic mess we’re trying to avoid in the first place with
containers.

In theory, we should be able to collocate the stateless apps with the
data management services they rely on, with all of these services
running in containers, but Docker isn’t there quite yet and this
quickly becomes a whole different ballgame which Docker itself does
not touch. ClusterHQ is attempting to tackle this problem for
containers with their tool Flocker, by using ZFS as the foundation for
data management and replication. However, Flocker is still in the very
early stages from being practical, and it assumes you are accepting of
a clustered filesystem model and the nuances it can introduce such as
relying on a storage pool, and not to mention I/O taking a performance
hit. However, Flocker is open source and free to use if you care to
kick the tires and ClusterHQ seems to be making serious waves as a
leader on this front, so keeping tabs on them is well-advised.

For Docker to truly flourish as the new infrastructure stack that
powers the next web, containers need serious work mitigating the data
management problem. For now, it is suggested that you stick to the
traditional data management solutions that live outside of containers,
or strictly use Data volumes as a means to map to a host directory or
a file from a container, that you can curate and manage external to
Docker.

Block Storage
-------------

Given the previous section’s description of Docker volumes to manage
your data, and the assumption that you are operating on a cloud
provider, it is only logical to think that one could use the
provider’s block storage as a service in conjunction with volume
mounts, if you decided to go down that route.

In theory, this sounds great; however, you could imagine the scenario
where you have several hundred containers running on a host, all
requiring an allocated & attached block device to mount their
individual volumes. In such a setting it is only a matter of time
before you hit a limit of some sort at the account level, or more
restrictively, at the service provider’s infrastructure level – both
of which are not novel issues you would have run into if you were
running containers or not.

However, you do have the option to map a Data volume container to a
block storage device and share said container with other containers without having
to introduce an additional management layer.
