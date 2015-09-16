---
title: Porting Your Application
slug: porting-your-application
description: How to port your existing application to RCS
topics:
  - docker
  - intermediate
---
By now you're beginning to understand Docker and what it can bring to your workflow and deployments. In this tutorial we will walk you through porting an existing Rails application to docker-compose and RCS. This application includes the Rails app its self along with Postgresql, Redis, and Sidekiq workers.

# docker-compose
`docker-compose` (formerly `fig`) is a Docker tool that allows you to script multiple containers that make up your application. It is expressed in simple YAML markup.

## Create Dockerfile

The `Dockerfile` is at the heart of porting your application. This file tells Docker how to behave and what commands to run and packages to install to your container.

    # Dockerfile
    FROM ubuntu:14.04
    RUN apt-get update -qq
    RUN apt-get install -y build-essential ruby-full nodejs npm nodejs-legacy libpq-dev vim curl patch gawk g++ gcc make libc6-dev patch libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev automake libtool bison pkg-config libffi-dev
    RUN gem install bundler

    RUN npm install -g phantomjs
    RUN mkdir /myapp

    WORKDIR /tmp
    COPY Gemfile Gemfile
    COPY Gemfile.lock Gemfile.lock
    RUN bundle install

    ADD . /myapp
    WORKDIR /myapp

## Create docker-compose.yml

The `docker-compose.yml` file is where you describe your application to Docker. In this instance we will need a database container (Postgresql) and the official image requires us to open port 5432, the standard Postgresql port. The `db` key naming is important -- it can be whatever you'd like to name it, but Docker will use this name to refer to the container later on.

Redis follows much the same way. Since these are "utility" containers we can just use the official image and expose a port so that our application can interact. The default Redis is 6379.

The `sidekiq` container is a bit different. Since we need to start the Sidekiq daemon on the container we have to tell it via the `command` key. Another key difference is the `links` key. This tells the `sidekiq` container that it should interact with the `db` and `redis` containers. This is where the naming of the containers comes into play -- the only way `sidekiq` container knows about `db` is because that's what you named the container. Docker will take care of all the internal networking and port forwarding simply by telling a container that it should interact with other containers.

Now that we have the "utility" containers ready it's time to define our own web application container. Under the `web` key we see a few new commands. The first new command is `build .` This essentially says that we're not using a base image and that we will supply the code needed for the container; since this is `.` it understands that the code to be placed is our own Rails app. The `command` key tells the container how to start the webserver of choice. The `volumes` command tells the container that it should mount the current directory (our Rails app) in the container at `/myapp`. The `ports` key asks that the container forward it's port 80 to our localhost's port 80. We see the `links` key once again. Our web server container will definitely need to talk to our `db` and `redis` containers. The last key, `env_file` is a way to specify environment variables. In this example only `web` has a file like this, but other containers may as well according to your needs.

    # docker-compose.yml
    db:
      image: postgres
      ports:
        - "5432"
    redis:
      image: redis
      ports:
        - "6379"
    sidekiq:
      build: .
      command: bundle exec sidekiq
      links:
        - db
        - redis
    web:
      build: .
      command: bundle exec thin start
      volumes:
        - .:/myapp
      ports:
        - "80:80"
      links:
        - db
        - redis
      env_file:
        - '.env.web'

## Example .env.web file
    LANG=en_US.UTF-8
    RACK_ENV=development
    RAILS_ENV=development
    RAILS_SERVE_STATIC_FILES=enabled
    FOO=bar

## Local bootstrap
Docker commands tend to be wordy so I like to create a script that will start a development environment for other team members. A script located at `RAILS_ROOT/bin/start_dev_env` does the trick:

    #!/usr/bin/env bash

    # Assume Docker Toolbox (https://www.docker.com/toolbox)

    docker-compose build
    open https://$(boot2docker ip)
    docker-compose up

## RCS
