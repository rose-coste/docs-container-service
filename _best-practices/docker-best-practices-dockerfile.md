---
title: Docker best practices: Dockerfile
slug: docker-best-practices-dockerfile
description: Docker best practices, powered by the Rackspace Container Service
author: Mike Metral, <mike.metral@rackspace.com>, Rackspace Product Architect
topics:
  - docker
  - beginner
---

Docker best practices: Dockerfile
=================================

To build a Docker image, application and dependency specifics aside,
there are two primary methods to do so:

-   Update a container created from an image and commit the changes to an image

-   Use a Dockerfile

The update-and-commit route requires that one run a previously created
Docker image that you’ve retrieved from either a public or private
image registry and then instantiated the container on your host
running the Docker daemon, preferably with an activated TTY. Once you
have access to the container, you can make the required changes to
personalize it. Upon completion, you exit the container and use
\`docker commit\` to commit a copy of the container to your local or
remote registry.

Using the update-and-commit methodology is the simplest way of an
extending an image, but its not easy to maintain or share and it is
definitely not the cleanest. Going the Dockerfile route is preferred
as doing so allows you to utilize the \`docker build\` command to
build images from scratch. In a Dockerfile you can prescribe the base
OS for the container, in addendum to the instructions you’d like to
execute inside the image from installing packages to setting &
configuring options.

Sample Dockerfile
-----------------

Here is a snippet of a simple Dockerfile:

    # This is a comment
    FROM ubuntu:14.04
    MAINTAINER Kate Smith ksmith@example.com
    RUN apt-get update && apt-get install -y ruby ruby-dev
    RUN gem install sinatra

Each instruction creates a new layer of the image. At the time of this
report, an image cannot have more than 127 layers regardless of the
storage driver. This limitation is set globally to encourage
optimization of the overall size of images (28).

It is worth nothing that the base OS initially chosen
tends to be a familiar OS that one is most accustomed to working with
such as Ubuntu, CentOS, or Fedora.

However, it becomes evident that most of the interaction with the actual
OS revolves around the filesystem layout and its binaries. For example,
if you chose Ubuntu as your base OS, and you aren’t doing anything super
extravagant in terms of the OS itself, then you could potentially switch
to the Debian image as it will most likely have everything you need and
can save you at least 100MB+ in size. Again, it is on a per case basis, but
if you’re utilizing a bloated OS base when BusyBox could suffice, then
you’re not only consuming more space than needed, you’re also adding
time to Docker builds and Image Repository interaction.

Recommendations
---------------

The following is a collection of unordered recommendations and tips
that don’t get enough upfront attention when starting off with Docker.
One should keep these tips in mind so that you can ultimately improve
creation and utilization of Dockerfiles to build your container images.

-   Use a .dockerignore file to exclude directories and/or files from
    the build process

-   Keep the number of instructions/layers to a minimum as this
    ultimately affects build performance and time

-   Consolidation of similar instructions into a single layer is
    preferred (i.e. in the snippet from the previous example,
    place the \`gem install sinatra\` in the preceding line.

-   Build cache: During the process of building an image Docker will
    step through the instructions in your Dockerfile executing each in
    the order specified. As each instruction is examined Docker will
    look for an existing image in its cache that it can reuse, rather
    than creating a new (duplicate) image

-   In the case of the ADD and COPY instructions, the contents of
    the file(s) being put into the image are examined.
    Specifically, a checksum is done of the file(s) and then that
    checksum is used during the cache lookup. If anything has
    changed in the file(s), including its metadata, then the cache
    is invalidated.

-   Aside from the ADD and COPY commands, cache checking will not
    look at the files in the container to determine a cache match.
    For example, when processing a \`RUN apt-get -y\` update
    command the files updated in the container will not be
    examined to determine if a cache hit exists. In that case just
    the command string itself will be used to find a match (29).

-   When testing an edit to your codebase, write a simple Dockerfile
    describing your build process (usually in 5 to 10 lines), then
    build a new container from source with \`docker build\`

-   Building is very fast because docker re-­‐uses previous build
    steps whenever possible (see preceding "Build cache" tip). And
    by building every time, you can use containers as reliable artifacts. For example,
    you can go back and run the container from four changes ago to inspect a
    problem, or run long tests on a version while editing the code.

-   For a development environment, map your source code on the host to
    container using a volume so that way you can use your editor of
    choice on the host and test right away in the container.
    This is done by mounting the current folder as a volume
    **rather** than using the ADD command in the Dockerfile

-   Know the Differences Between CMD and ENTRYPOINT. There are many details
    of how the Dockerfile CMD and ENTRYPOINT
    instructions work, and expressing exactly what you want is key
    to ensuring users of your image have the right experience.
    These two instructions are similar in that they both specify
    commands that run in an image, but there’s an important
    difference: CMD simply sets a command to run in the image if
    no arguments are passed to \`docker run\`, while ENTRYPOINT is
    meant to make your image behave like a binary. The rules are
    essentially:

-- If your Dockerfile uses only CMD, the provided command will be run
   if no arguments are passed to \`docker run\`.

-- If your Dockerfile uses only ENTRYPOINT, the arguments passed to
   docker run will always be passed to the entrypoint; the entrypoint
   will be run if no arguments are passed to \`docker run\`.

-- If your Dockerfile declares both ENTRYPOINT and CMD, and no arguments are passed to \`docker run\`, then the argument(s) to CMD will be passed to the declared entrypoint.

-- Be careful with using ENTRYPOINT; it will make it more difficult to
   get a shell inside your image. While it may not be an issue if your
   image is designed to be used as a single command, it can frustrate or
   confuse users that expect to be able to use the idiom (30).

For further best practices and the full set of Docker’s instructions,
visit
<https://docs.docker.com/articles/dockerfile\_best-­‐practices/>.
