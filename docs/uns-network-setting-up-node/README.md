---
home: false
title: "Setting-up your uns.network Node"
---

# Setting-up your Node

[[toc]]

## Introduction

Here we will help you set up a <uns/> relay node and/or forger node.

To make your life easier, we've built Docker images of <uns/> nodes.

### What is docker 

Docker is the de facto industry standard for packaging applications into a container. By doing so, all dependencies, such as the language runtimes, operating system, and libraries are combined with the product.

Different cloud providers offer specific products to host your Docker containers, such as:

- [AWS Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html)
- [Azure](https://azure.microsoft.com/en-us/services/kubernetes-service/docker/)
- [Digital Ocean](https://www.digitalocean.com/products/one-click-apps/docker/)
- [Scaleway](https://www.scaleway.com/en/)
- [OVH](https://www.ovh.com)

Orchestrators with Docker as a first-class citizen:

- [Kubernetes](https://kubernetes.io/)
- [Nomad](https://www.nomadproject.io/)
- [Mesos](http://mesos.apache.org/)

### Minimum requirements for running a SANDBOX relay or forger node

- Linux: Ubuntu 16.04 / 18.04, CentOS/RHEL 7 / 8 ...
- 2+ vCPU x86/64 bits
- 2-4 GB RAM
- at least 20 GB drive 

We strongly recommend running your node on SSD drive as there are a lot of read and write operations to the DB.

## Installation

First of all, be sure to have these prerequisites:
- [Docker](https://docs.docker.com/install) installed on your Linux machine
- [Docker Compose](https://docs.docker.com/compose/install) installed on your machine (only if you want to use it to setup node, documented below).
- a [Docker Hub](https://hub.docker.com/) account (Sign up if you don't have one yet).

## Network configuration

Depending on your hosting infrastructure, you will need open TCP ports on your firewall:
- SANDBOX:
  * `4102` (required): this is the communication port used by the node to exchange information with other nodes of the uns.network
  * `4103` (optional): open it if you want to open the API to the world, in order to submit transactions for example. If you don't know what is it for, keep it closed
- LIVENET: not available yet

::: warning
Don't forget to forward (by NAT, routing ...) the opened ports to the corresponding ports on your VM or Docker engine!
:::

Unfortunately, there is no standard documentation to do that, so please follow the configuration guide of the firewall provided by your hosting provider, or if you are running a node at home, the guide for your 'home box'.

## Docker configuration

Docker CLI must be logged with your docker account (using command [`login`](https://docs.docker.com/engine/reference/commandline/login/))

Currently, our network is in **private alpha mode** so we've deployed docker image to a private repository into our organization.

It means that our code is only accessible via invitation. [Send us](mailto:contact@unik-name.com) your docker ID or email in order to grant you read access. 

Once done, you'll be able to pull our `universalnamesystem/core` image and connect to the Docker hub:

```shell
$ docker login
```

## Run a node with Docker Compose

In this documentation, we'll start sandbox <uns/> node using [Docker Compose](https://docs.docker.com/compose/) and a configuration file.

Create the following configuration file `docker-compose.yml`: 

```docker
version: '2'
services:
  postgres:
    image: "postgres:11-alpine"
    container_name: postgres-sandbox
    restart: always
    ports:
      - '127.0.0.1:5432:5432'
    volumes:
      - 'postgres:/var/lib/postgresql/data'
    networks:
      - core
    environment:
     POSTGRES_PASSWORD: password
     POSTGRES_DB: uns_sandbox
     POSTGRES_USER: uns

  uns:
    image: universalnamesystem/core:sandbox
    container_name: uns
    restart: always
    environment:
     UNS_NET: sandbox
     BOOTSTRAP: "true"
     # FORGER_SECRET: "" # <-- Edit here to start a forger. Do nothing to start a relay
     CORE_DB_HOST: postgres
     CORE_DB_PORT: 5432
     CORE_DB_PASSWORD: password
     CORE_DB_DATABASE: uns_sandbox
     CORE_DB_USER: uns
    ports:
     - "4002:4002"
     - "4003:4003"
    cap_add:
      - SYS_NICE
      - SYS_RESOURCE
      - SYS_TIME
    networks:
      - core
    tty: true
    links:
     - postgres
    depends_on:
      - postgres
volumes:
  postgres:
networks:
  core:
```

This file really ease node start, configuring it for you. 

::: tip
You can run a node without docker-compose with simple docker commands, but we won't describe here.
:::

::: tip
**Optionally**, you can get access to log and configuration files, by mapping a volume to `/etc/uns` and `/var/log/uns`.
:::

### Explanations

From configuration file we can see that 2 services will be started: a node and its database.

You also have 2 volumes mounted on the file system (for node and database files) and a local network for services communication.

#### The database service

Based on [`postgres` (version 11 and Linux alpine)](https://github.com/docker-library/postgres/blob/0a66d53fface5ccc8274f99712ba2f382a1caf42/11/alpine/Dockerfile), it exposes a single port (`5432`) and requires environment variables (for db name, user name and password).
These variables must match with those provided in the node service (see below).

#### The node service

The node service is based on our `universalnamesystem/core` image (latest tag). 

It has two exposed ports; for p2p (`4102`) and for API (`4103`), and mount 3 volumes (for logs, forger secret and configuration files).

Some environment variables are set :
- for the database (port, user name, user password and db name)
- for the network (here it's `sandbox`, it means that it'll connect to other <uns/> sandbox nodes)
- and the forger secret (line `FORGER_SECRET`, commented by default). 

### Run a relay with Docker Compose

With the previously created configuration file, you simply have to run:

```shell
$ docker-compose up
```

The node starts logging a lot of information and tries to reach peers before syncing.
You can stop the node by hitting `CTRL+C`.


### Run a permanent relay with Docker Compose

If you want to permanently run your node (so as a **d**aemon), use this command:

```shell
$ docker-compose up -d
```

Then you can shutdown it with:

```shell
$ docker-compose down
```

### Run a forger with Docker Compose

::: warning
You must be a [declared delegate](/uns-network-player/#becoming-a-delegate) before doing this configuration.
:::

To run a forger node, please edit the `docker-compose.yml` file.

Find and uncomment line `FORGER_SECRET` and set your wallet passphrase as value. Then, start your forger node with the same commands as for a relay node (see [Run a relay with Docker Compose](#run-a-relay-with-docker-compose) above).

Now you have a running relay node and you're ready to forge blocks.
