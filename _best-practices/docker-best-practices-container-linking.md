---
title: Docker best practices: container linking
slug: docker-best-practices-container-linking
description: Docker best practices, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

# Docker best practices: container linking

Docker has a concept known as “linking” that allows you to connect
containers via a socket or through a hostname using a sender and
recipient model. Links can also be used to leverage service discovery
between containers. The way links function is by having the client
container use the private networking interface provided by Dockers to
access an exposed port in another server container.

For example: to create a database backend server that will be linked,
you start it just like any other container, with \`docker run -d --name mysql\_server mysql \`.

On the client container, you create the container as follows using an
alias for the serving container’s name:

**\`**docker run --rm -t -i --name webapp --link mysql\_server:db mywebpp:v1 /bin/bash**\`**

With linking established, the client is now allowed to access the
information on the serving container. This access is facilitated by
links in the form of a secure tunnel that is created between
containers and the connectivity information is exposed to the client
container in two ways: environmental variables, and via the /etc/hosts
file.

In the environmental variable route, the client will see information
such as the following:

- DB\_NAME=/webapp/db

- DB\_PORT=tcp://172.17.0.5:3306

- DB\_PORT\_3306\_TCP=tcp://172.17.0.5: 3306

- DB\_PORT\_3306\_TCP\_PROTO=tcp

- DB\_PORT\_3306\_TCP\_PORT=3306

- DB\_PORT\_3306\_TCP\_ADDR=172.17.0.5

In addition to the environment variables, Docker adds a host entry for
the source container to the /etc/hosts file.

## Ambassador pattern

In Docker linking, a pattern emerged that allows the proxying of
connections between a server and client container. This separate proxy
container transparently redirects connections based on parameters.

However, because ambassador containers themselves depend on links, they
too are exposed to the issues linking has as described in the proceeding
section, primarily, that it is only beneficial to use it as long as it
does not fail and/or crash.

In essence, the usage of the ambassador pattern has dwindled down as far
as the community is concerned as it’s not necessarily adding any benefit
that links don’t already provide, and it further complicates the
architecture without solving the underlying issues at hand.

## Issues with Linking

As provocative as linking seems, it seems to be met with a couple of
different nuances and issues when you begin to think of linking
containers across different hosts as well as how much of an ephemeral
lifecycle a particular container can hold.

Some problems and needs not addressed by links include:

-   Service discovery suffers from the static nature of linking.

-   Links are volatile. IP addresses, port mappings, link names can
    change as the result of manipulating a link, and other containers
    are not notified of these changes nor can they trivially deal with
    these issues (33).

With such issues, it becomes hard to use containers with links when
certain impositions exist. Links functionality can only work for
containers on a single / same node or by using the ambassador model,
which itself isn’t being adopted much.

Additionally, the exposure of the environmental variables in linking is
done in a very odd manner (reference the previous section’s example):
one must know know the intended interface and the port of the service
set in the environment beforehand, which itself is supposed to be
handling the task of supplying you with what the interface and port
*are* – a redundant and quite useless feature. For example, if one cares
to know what protocol is being used in the connection such as described
in the previous section’s example of the environmental variables, which
is TCP, one must know that the environmental variable is named
DB\_PORT\_3306\_TCP\_PROTO.

Also, once you discover the environmental variables, you’ll have to
parse the various strings for each connection to compose the full
connection information, so you may as well just have a single
environment variable containing all the host/port/interface information,
such as the following:

- DB\_NAME=/web2/db

- DB\_PORTS=tcp://172.17.0.5:3306,udp://172.17.0.5: 3306

In summary, links are not at the state to easily aid the developer in
terms of connecting server and client containers without much
premonition of the connection itself, and its stability and benefits
just aren’t ready to be integrated into
mission-critical/production-grade stacks.

An alternative that is becoming an industry standard is to use a
service registration and discovery tool. You can read more about this at
[Introduction to container technologies: registration and discovery of container services] (/container-technolgies-registration-discover/).

## Resources

For detailed background information about Docker best practices
for the Rackspace Container Service,
follow the links suggested at
[Introduction to container ecosystems and technologies: suggested reading]
(/container-technologies-references/).
