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

4. Download latest DEB package

https://desktop.docker.com/linux/main/amd64/docker-desktop-4.16.2-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64

5. Install the package with apt as follows

```sh
$ sudo apt-get update
$ sudo apt-get install ~/Downloads/docker-desktop-4.16.2-amd64.deb
...
N: Der Download wird als root und nicht Sandbox-geschützt durchgeführt, da auf die Datei »/home/ralf/Downloads/docker-desktop-4.16.2-amd64.deb« durch den Benutzer »_apt« nicht zugegriffen werden kann. - pkgAcquire::Run (13: Keine Berechtigung)
```

At the end of the installation process, apt displays an error due to installing a downloaded package. You can ignore this error message.

There are a few post-install configuration steps done through the post-install script contained in the deb package.

The post-install script:

* Sets the capability on the Docker Desktop binary to map privileged ports and set resource limits.
* Adds a DNS name for Kubernetes to /etc/hosts.
* Creates a link from /usr/bin/docker to /usr/local/bin/com.docker.cli

To start Docker Desktop for Linux, search Docker Desktop on the Applications menu and open it. This launches the Docker menu icon and opens the Docker Dashboard, reporting the status of Docker Desktop.

To enable Docker Desktop to start on login, from the Docker menu, open a terminal and run:

```sh
$ systemctl --user enable docker-desktop
Created symlink /home/ralf/.config/systemd/user/docker-desktop.service → /usr/lib/systemd/user/docker-desktop.service.
Created symlink /home/ralf/.config/systemd/user/graphical-session.target.wants/docker-desktop.service → /usr/lib/systemd/user/docker-desktop.service.
```

To stop Docker Desktop, open a terminal and run:

```sh
$ systemctl --user stop docker-desktop
```

Install docker-compose:

```sh
$ sudo apt-get install docker-compose
```

Install docker engine

```sh
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

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