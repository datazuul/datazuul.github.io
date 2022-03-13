---
layout: post
title: Linux Usermanagement
author: Ralf Eichinger
toc: true
---

# Create default home directory for existing user

```sh
$ sudo mkdir /home/testinguser
$ sudo chown testinguser:testinguser /home/testinguser
$ sudo usermod -d /home/testinguser testinguser
```

# sudo

## Allow user to change to another user

Edit /etc/sudoers:

```sh
# visudo
ralf ALL=(postgres) ALL
```

Test it

```sh
ralf$ sudo -su postgres
```

or

```sh
ralf$ sudo -u postgres -i
```