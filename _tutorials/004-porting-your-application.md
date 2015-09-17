---
title: Porting Your Application
slug: porting-your-application
description: How to port your existing application to RCS
topics:
  - docker
  - intermediate
---
By now you're beginning to understand Docker and what it can bring to your workflow and deployments. In this tutorial we will walk you through porting an existing Rails application to docker-compose and RCS. This application includes the Rails app its self along with Postgresql, Redis, and Sidekiq workers.

`docker run --name db -e POSTGRES_PASSWORD=mysecretpassword -d postgres`

`docker run --name redis -d redis`

`docker build -t myapp .`

`docker run -d --link db:postgres --link redis:redis -p 3000:3000 myapp bundle exec thin start`

`docker run -d --link db:postgres --link redis:redis myapp bundle exec sidekiq`




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

    ENV POSTGRES_USER postgres
    ENV POSTGRES_PASSWORD mysecretpassword


## Update database.yml
    development: &default
      adapter: postgresql
      encoding: unicode
      database: myapp_dev
      pool: 5
      username: <%= ENV['POSTGRES_USER'] %>
      password: <%= ENV['POSTGRES_PASSWORD'] %>
      host: db

    <!-- test:
      <<: *default
      database: myapp_test

    production:
      <<: *default
      database: myapp_prod -->


## Local bootstrap
Docker commands tend to be wordy so I like to create a script that will start a development environment for other team members. A script located at `RAILS_ROOT/bin/start_dev_env` does the trick:

    #!/usr/bin/env bash

    # Assume Docker Toolbox (https://www.docker.com/toolbox)

    # Create a VM specifically for this project, named "porting"
    # docker-machine create --driver virtualbox porting

    # Load environment variables from Docker (links our shell to the porting VM)
    eval "$(docker-machine env porting)"

    # Let's remove all containers so we start with a blank slate
    docker rm --force `docker ps -qa`

    # Run the Postgresql container with a password (ensure update of database.yml)
    docker run --name db -e POSTGRES_PASSWORD=mysecretpassword -d postgres

    # Run a Redis container for Sidekiq
    docker run --name redis -d redis

    # Build a container from this Rails app (using Dockerfile)
    docker build -t myapp .

    # Run database operations in a utility container
    docker run -d --link db:postgres --link redis:redis myapp rake db:create db:schema:load db:migrate db:seed

    # Run the Sidekiq process in it's own container
    docker run -d --link db:postgres --link redis:redis myapp bundle exec sidekiq

    # Start our Rails app on port 3000, and link redis and postgresql to the app.
    docker run -d --link db:postgres --link redis:redis -p 3000:3000 myapp bundle exec thin start

    # Shows a list of our containers in the porting machine
    docker ps -a

    # Open our web app in the browser
    open http://$(dm ip porting):3000


## Rackspace bootstrap

    #!/usr/bin/env bash

    # Assume Docker Toolbox (https://www.docker.com/toolbox)

    # Create a VM specifically for this project, named "raxhost"
    docker-machine create --driver rackspace \
                      --rackspace-api-key $YOUR_API_KEY \
                      --rackspace-username $YOUR_USERNAME \
                      --rackspace-region DFW raxhost

    # Load environment variables from Docker (links our shell to the raxhost VM)
    eval "$(docker-machine env raxhost)"

    # Let's remove all containers so we start with a blank slate
    docker rm --force `docker ps -qa`

    # Run the Postgresql container with a password (ensure update of database.yml)
    docker run --name db -e POSTGRES_PASSWORD=mysecretpassword -d postgres

    # Run a Redis container for Sidekiq
    docker run --name redis -d redis

    # Build a container from this Rails app (using Dockerfile)
    docker build -t myapp .

    # Run database operations in a utility container
    docker run -d --link db:postgres --link redis:redis myapp rake db:create db:schema:load db:migrate db:seed

    # Run the Sidekiq process in it's own container
    docker run -d --link db:postgres --link redis:redis myapp bundle exec sidekiq

    # Start our Rails app on port 3000, and link redis and postgresql to the app.
    docker run -d --link db:postgres --link redis:redis -p 3000:3000 myapp bundle exec thin start

    # Shows a list of our containers in the raxhost machine
    docker ps -a

    # Open our web app in the browser
    open http://$(dm ip raxhost):3000

## RCS
