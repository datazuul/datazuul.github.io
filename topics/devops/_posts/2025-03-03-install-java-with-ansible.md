---
layout: post
title: Automated installation of Java with Ansible
author: Ralf Eichinger
toc: true
---

In our previous post [Automated administration with Ansible]({% post_url topics/devops/2025-03-03-ansible %) we installed Ansible for automated host administration.

Let's use Ansible to install Java on the managed hosts.

We will write a playbook to use roles to install java on multiple managed host machines.

# Prerequisites

First check setup of remote users on hosts:

```sh
$ cd mycompany-devops/ansible

$ ansible all -m command -a 'id' -i inventory.yml 
new_server | CHANGED | rc=0 >>
uid=1000(remote_username) gid=1000(remote_username) groups=1000(remote_username),27(sudo)
old_server | CHANGED | rc=0 >>
uid=1000(remote_username) gid=1000(remote_username) Gruppen=1000(remote_username),27(sudo)
```

So the user is in `sudo` group on all hosts. (As we see later this will not be sufficient ...)

Check operating system on hosts:

```sh
$ ansible all -m setup -i inventory.yml | grep family
        "ansible_os_family": "Debian",
        "ansible_os_family": "Debian",
```

# Write Playbook to install OpenJDK 21

As we manage Debian hosts we can use `apt` to install OpenJDK 21.

Playbook `mycompany-devops/ansible/playbooks/openjdk-playbook.yaml`:

```yml
- name: Install Java
  hosts: all
  tasks:
    - name: Install OpenJDK 21
      apt:
        name: openjdk-21-jdk
        state: present
```

Run the playbook:

```sh
$ ansible-playbook -i inventory.yml playbooks/openjdk-playbook.yaml

PLAY [Install Java] ************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************
ok: [old_server]
ok: [new_server]

TASK [Install OpenJDK 21] ******************************************************************************************************************************************
[WARNING]: Updating cache and auto-installing missing dependency: python3-apt
fatal: [old_server]: FAILED! => {"changed": false, "cmd": "/usr/bin/apt-get update", "msg": "E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)\nE: Unable to lock directory /var/lib/apt/lists/\nW: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)\nW: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)", "rc": 100, "stderr": "E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)\nE: Unable to lock directory /var/lib/apt/lists/\nW: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)\nW: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)\n", "stderr_lines": ["E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)", "E: Unable to lock directory /var/lib/apt/lists/", "W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)", "W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)"], "stdout": "Reading package lists...\n", "stdout_lines": ["Reading package lists..."]}
fatal: [new_server]: FAILED! => {"changed": false, "msg": "No package matching 'openjdk-21-jdk' is available"}

PLAY RECAP *********************************************************************************************************************************************************
new_server                 : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0   
old_server                 : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0  
```

The installation fails as the remote user lacks root privileges ("Permission denied").

We configure the `sudo` password for the hosts in our inventory. Add `ansible_become` vars to each host (excerpt):

```yml
all:
  hosts:
    old_server:
      ansible_become: true
      ansible_become_method: sudo
      ansible_become_password: 'password'
    new_server:
      ansible_become: true
      ansible_become_method: sudo
      ansible_become_password: 'password'
```

We get error

```
fatal: [new_server]: FAILED! => {"changed": false, "msg": "No package matching 'openjdk-21-jdk' is available"}
```

So we change our playbook name to `default-jre-playbook.yaml` and to install package `default-jre`:

```yml
- name: Install Java Runtime Environment
  hosts: all
  tasks:
    - name: Install default JRE
      apt:
        name: default-jre
        state: present
```

Run playbook:

```sh
$ ansible-playbook -i inventory.yml playbooks/default-jre-playbook.yaml

PLAY [Install Java] ************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************
ok: [old_server]
ok: [new_server]

TASK [Install OpenJDK 21] ******************************************************************************************************************************************
[WARNING]: Updating cache and auto-installing missing dependency: python3-apt
fatal: [old_server]: FAILED! => {"changed": false, "cmd": "/usr/bin/apt-get update", "msg": "E: The repository 'http://apt.postgresql.org/pub/repos/apt buster-pgdg Release' no longer has a Release file.\nW: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: https://nginx.org/packages/mainline/debian buster InRelease:********@nginx.com>\nE: Failed to fetch https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/dists/buster/InRelease  Clearsigned file isn't valid, got 'NOSPLIT' (does the network require authentication?)\nE: The repository 'https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease' is no longer signed.", "rc": 100, "stderr": "E: The repository 'http://apt.postgresql.org/pub/repos/apt buster-pgdg Release' no longer has a Release file.\nW: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: https://nginx.org/packages/mainline/debian buster InRelease: The following signatures were invalid: EXPKEYSIG ABF5BD827BD9BF62 nginx signing key <signing-key@nginx.com>\nE: Failed to fetch https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/dists/buster/InRelease  Clearsigned file isn't valid, got 'NOSPLIT' (does the network require authentication?)\nE: The repository 'https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease' is no longer signed.\n", "stderr_lines": ["E: The repository 'http://apt.postgresql.org/pub/repos/apt buster-pgdg Release' no longer has a Release file.", "W: An error occurred during the signature verification. The repository is not updated and the previous index files will be used. GPG error: https://nginx.org/packages/mainline/debian buster InRelease: The following signatures were invalid: EXPKEYSIG ABF5BD827BD9BF62 nginx signing key <signing-key@nginx.com>", "E: Failed to fetch https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/dists/buster/InRelease  Clearsigned file isn't valid, got 'NOSPLIT' (does the network require authentication?)", "E: The repository 'https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease' is no longer signed."], "stdout": "Hit:1 https://nginx.org/packages/mainline/debian buster InRelease\nHit:2 http://security.debian.org buster/updates InRelease\nIgn:3 http://apt.postgresql.org/pub/repos/apt buster-pgdg InRelease\nHit:4 http://ftp.debian.org/debian buster InRelease\nErr:5 http://apt.postgresql.org/pub/repos/apt buster-pgdg Release\n  404  Not Found [IP: 147.75.85.69 80]\nHit:6 http://ftp.debian.org/debian buster-updates InRelease\nErr:1 https://nginx.org/packages/mainline/debian buster InRelease\n  The following signatures were invalid: EXPKEYSIG ABF5BD827BD9BF62 nginx signing key <signing-key@nginx.com>\nGet:7 https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease [11.5 kB]\nErr:7 https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease\n  Clearsigned file isn't valid, got 'NOSPLIT' (does the network require authentication?)\nReading package lists...\n", "stdout_lines": ["Hit:1 https://nginx.org/packages/mainline/debian buster InRelease", "Hit:2 http://security.debian.org buster/updates InRelease", "Ign:3 http://apt.postgresql.org/pub/repos/apt buster-pgdg InRelease", "Hit:4 http://ftp.debian.org/debian buster InRelease", "Err:5 http://apt.postgresql.org/pub/repos/apt buster-pgdg Release", "  404  Not Found [IP: 147.75.85.69 80]", "Hit:6 http://ftp.debian.org/debian buster-updates InRelease", "Err:1 https://nginx.org/packages/mainline/debian buster InRelease", "  The following signatures were invalid: EXPKEYSIG ABF5BD827BD9BF62 nginx signing key <signing-key@nginx.com>", "Get:7 https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease [11.5 kB]", "Err:7 https://adoptopenjdk.jfrog.io/adoptopenjdk/deb buster InRelease", "  Clearsigned file isn't valid, got 'NOSPLIT' (does the network require authentication?)", "Reading package lists..."]}
changed: [new_server]

PLAY RECAP *********************************************************************************************************************************************************
new_server                 : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
old_server                 : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0 
```

The installation was successful on `new_server` but failed on `old_server` because of misconfiguration.

Let's check the installed java:

```sh
$ ansible new_server -m command -a 'java --version' -i inventory.yml 
new_server | CHANGED | rc=0 >>
openjdk 17.0.14 2025-01-21
OpenJDK Runtime Environment (build 17.0.14+7-Debian-1deb12u1)
OpenJDK 64-Bit Server VM (build 17.0.14+7-Debian-1deb12u1, mixed mode, sharing)
```

So Java 17 is installed successfully on `new_server`.
