---
layout: post
title: Docker Desktop & Docker Compose
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://docs.docker.com/>
* Documentation: <https://docs.docker.com/desktop/install/ubuntu/>

# Installation

It is recommended to install "Docker Desktop".

## Ubuntu

1. Add Docker's official GPG key

```shell
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl gnupg
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

2. Add the repository to Apt sources

```shell
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update
```

3. Install Docker packages:

```shell
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

4. Install Docker Desktop:

Download latest DEB package from <https://docs.docker.com/desktop/install/ubuntu/>: <https://desktop.docker.com/linux/main/amd64/docker-desktop-4.24.2-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64>

5. Install the package with apt as follows:

```shell
$ sudo apt-get update
$ sudo apt-get install ~/Downloads/docker-desktop-4.24.2-amd64.deb
```

See <https://docs.docker.com/desktop/get-started/>:

Docker recommends that you authenticate using the Sign in option in the top-right corner of the Docker Dashboard.
Once signed in, you can access your Docker Hub repositories directly from Docker Desktop.

Install `pass` credentials storage. Docker Desktop relies on pass to store credentials
in gpg2-encrypted files. Before signing in to Docker Hub from the Docker Dashboard or the Docker menu,
you must initialize pass. Docker Desktop displays a warning if you've not initialized pass.

You can initialize pass by using a gpg key. To generate a gpg key, run:

```shell
$ gpg --generate-key
gpg (GnuPG) 2.2.40; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: Verzeichnis `/home/ralf/.gnupg' erzeugt
gpg: Die "Keybox" `/home/ralf/.gnupg/pubring.kbx' wurde erstellt
Hinweis: "gpg --full-generate-key" ruft den erweiterten Dialog auf.

GnuPG erstellt eine User-ID, um Ihren Schlüssel identifizierbar zu machen.

Ihr Name: Hugo Chef
Email-Adresse: lala@gmail.com
Sie haben diese User-ID gewählt:
    "Hugo Chef <lala@gmail.com>"

Ändern: (N)ame, (E)-Mail oder (F)ertig/(A)bbrechen? F
Wir müssen eine ganze Menge Zufallswerte erzeugen.  Sie können dies
unterstützen, indem Sie z.B. in einem anderen Fenster/Konsole irgendetwas
tippen, die Maus verwenden oder irgendwelche anderen Programme benutzen.
Wir müssen eine ganze Menge Zufallswerte erzeugen.  Sie können dies
unterstützen, indem Sie z.B. in einem anderen Fenster/Konsole irgendetwas
tippen, die Maus verwenden oder irgendwelche anderen Programme benutzen.
gpg: /home/ralf/.gnupg/trustdb.gpg: trust-db erzeugt
gpg: Verzeichnis `/home/ralf/.gnupg/openpgp-revocs.d' erzeugt
gpg: Widerrufzertifikat wurde als '/home/ralf/.gnupg/openpgp-revocs.d/4F012AD9D556CE0A2DE10BA273CDB97E0FF73B46.rev' gespeichert.
Öffentlichen und geheimen Schlüssel erzeugt und signiert.

pub   rsa3072 2023-10-14 [SC] [verfällt: 2025-10-13]
      4F012AD9D556CE0A2DE10BA273CDB97E0FF73B46
uid                      Hugo Chef <lala@gmail.com>
sub   rsa3072 2023-10-14 [E] [verfällt: 2025-10-13]
```

To initialize pass, run the following command using the public key generated from the previous command:

```shell
$ pass init 4F012AD9D556CE0A2DE10BA273CDB97E0FF73B46
mkdir: Verzeichnis '/home/ralf/.password-store/' angelegt
Password store initialized for 4F012AD9D556CE0A2DE10BA273CDB97E0FF73B46
```
# Usage

## Docker Desktop

Everytime you need docker/docker compose start the Desktop.

You need to create an account at <https://hub.docker.com> for login.

Start screen:

![Docker Desktop start screen](/home/ralf/DEV/github.com/datazuul/datazuul.github.io/assets/topics/development/docker/docker-desktop-first-start.jpg)

Once you initialize pass, you can sign in on the Docker Dashboard and pull your private images.
When Docker CLI or Docker Desktop use credentials, a user prompt may pop up for the password
you set during the gpg key generation.

Example:

```shell
$ docker pull molly/privateimage
Using default tag: latest
latest: Pulling from molly/privateimage
3b9cc81c3203: Pull complete 
Digest: sha256:3c6b73ce467f04d4897d7a7439782721fd28ec9bf62ea2ad9e81a5fb7fb3ff96
Status: Downloaded newer image for molly/privateimage:latest
docker.io/molly/privateimage:latest
```
## Docker

Verify that the Docker Engine installation is successful by running the `hello-world` image:

```shell
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:88ec0acaa3ec199d3b7eaf73588f4518c25f9d34f58ce9a0df68429c5af48e8d
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

