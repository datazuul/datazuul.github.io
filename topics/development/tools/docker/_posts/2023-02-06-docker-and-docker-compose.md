---
layout: post
title: Docker & Docker Compose
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://docs.docker.com/>
* Documentation: <https://docs.docker.com/desktop/install/ubuntu/>

# Installation

It is recommended to install "Docker Desktop".

## Ubuntu 22.10

1. Set up Docker’s package repository

```sh
$ sudo apt-get update
$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2. Add Docker’s official GPG key

```sh
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

3. Use the following command to set up the repository

```sh
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Docker Engine & Docker Compose

Install docker engine

```sh
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

# Usage

## Docker

Run hello world:

```sh
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## Docker Compose

Use `docker compose` (without dash between...).

Start/Stop:

```sh
$ docker compose up -d
$ docker compose stop
```

Delete:

```sh
$ docker compose down
```

