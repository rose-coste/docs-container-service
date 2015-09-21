---
title: Docker best practices: image repository
slug: docker-best-practices-image-repository
description: Docker best practices, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Docker best practices: image repository
=======================================
Creating containers from saved container images enables you to
quickly create containers from a consistent starting point.
This is similar to the advantage that [Rackspace Cloud Images] (https://developer.rackspace.com/docs/user-guides/infrastructure/cloud-config/compute/cloud-images-product-concepts/image-properties/) provides when you
create [Rackspace Cloud Servers] (https://developer.rackspace.com/docs/user-guides/infrastructure/cloud-config/compute/cloud-servers-product-concepts/#cloud-servers-product-concepts): the setup process is predefined
and repeatable,
saving you from the potential delays and inconsistencies that are likely in
a manual, multi-step process.

You can create your containers from publicly-available images saved by the
community or from custom images you saved privately for your own reuse.

Community images
----------------

One of the main advantages of using Docker containers is that
the community has created
many Docker container images that you can use for your own purposes.
Many of the images created
by users have been uploaded to the Docker Hub Registry
(*https://registry.hub.docker.com/*) and this allows everyone to examine and use
them.

You can retrieve or pull an image by identifying the
\<user/image\_name\> and passing it to Docker. For example, \` docker pull
cpuguy83/openvpn\` asks for an image named `openvpn`,
owned by a user named `cpuguy83`.

In addition to images contributed by individual community members,
the Docker Hub Registry has official images from software providers, including
companies that provide operating systems.
Providers of official images include Ubuntu,
MySQL, Golang, and many others.
You can see the list of official repositories at
https://hub.docker.com/explore/.

You can pull official images with shortened calls.
For example, \`docker pull mysql\` asks for all official images
contributed by MySQL.

Personal images
---------------

You can upload your own custom images to several repositories, including
the [Docker Hub Registry] (https://registry.hub.docker.com/),
[CoreOS’ Quay.io] (https://quay.io/), or
your own private image registry available from a variety of providers.
Each image repository has its own method of
accepting contributions.
For example, to contribute an image to
the [Docker Hub Registry] (https://registry.hub.docker.com/),
log in using \`docker login\` and then issue
\`docker push \<user/image\_name\>\`.
You can also tag the image if you wish to give it a label
such as "dev", "production", or "v2" to distinguish among several flavors
of your image.

The default behavior for \`docker push\` is to push to the central
[Docker Hub Registry] (https://registry.hub.docker.com/), but if you prefer your own private
Docker registry there is an image for that too. Once you’ve got it
running, the only step that you are required to change is to login to
your private registry as opposed to the central one. That switch is as
easy as \`docker login http(s)://\<YOUR\_DOMAIN\>:\<PORT\>\`.
