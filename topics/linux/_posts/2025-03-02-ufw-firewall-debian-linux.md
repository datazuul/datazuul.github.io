---
layout: post
title: Configure iptables firewall with UFW under Debian Linux
author: Ralf Eichinger
toc: true
---

After installing a Debian Linux system on your server you should immediately secure the server with a firewall.

An easy to use interface to `iptables` firewall is `ufw` ("Uncomplicated Firewall", Homepage: [https://launchpad.net/ufw](https://launchpad.net/ufw)).

# Installation

```sh
sudo apt install ufw
...
Creating config file /etc/ufw/before.rules with new version
Creating config file /etc/ufw/before6.rules with new version
Creating config file /etc/ufw/after.rules with new version
Creating config file /etc/ufw/after6.rules with new version
Created symlink /etc/systemd/system/multi-user.target.wants/ufw.service → /lib/systemd/system/ufw.service.
...
```

Show help:

```sh
$ sudo ufw --help

Usage: ufw COMMAND

Commands:
 enable                          enables the firewall
 disable                         disables the firewall
 default ARG                     set default policy
 logging LEVEL                   set logging to LEVEL
 allow ARGS                      add allow rule
 deny ARGS                       add deny rule
 reject ARGS                     add reject rule
 limit ARGS                      add limit rule
 delete RULE|NUM                 delete RULE
 insert NUM RULE                 insert RULE at NUM
 prepend RULE                    prepend RULE
 route RULE                      add route RULE
 route delete RULE|NUM           delete route RULE
 route insert NUM RULE           insert route RULE at NUM
 reload                          reload firewall
 reset                           reset firewall
 status                          show firewall status
 status numbered                 show firewall status as numbered list of RULES
 status verbose                  show verbose firewall status
 show ARG                        show firewall report
 version                         display version information

Application profile commands:
 app list                        list application profiles
 app info PROFILE                show information on PROFILE
 app update PROFILE              update PROFILE
 app default ARG                 set default application policy
```

Show status:

```sh
$ sudo ufw status
Status: inactive
```

# Configuration

Make sure IPv6 is enabled:

```sh
$ sudo nano /etc/default/ufw
...
IPV6=yes
```

After the installation, UFW doesn’t have a default configuration. It is recommended to start by blocking all incoming traffic and allow outgoing traffic, and then open specific ports / services.

First of all we "close" the firewall by denying all incoming ports (not active by doing this unless firewall started/active/reloaded):

```sh
$ sudo ufw default deny incoming
Default incoming policy changed to 'deny'
(be sure to update your rules accordingly)
```

Set the default UFW outgoing policy to `allow` for all ports:

```sh
$ sudo ufw default allow outgoing
Default outgoing policy changed to 'allow'
(be sure to update your rules accordingly)
```

For our server we want to open incoming SSH (for administration), HTTP and HTTPS (for webserver):

```sh
$ sudo ufw allow ssh
Rules updated
Rules updated (v6)
$ sudo ufw allow http
Rules updated
Rules updated (v6)
$ sudo ufw allow https
Rules updated
Rules updated (v6)
```

Show rules added so far, even when the firewall is still disabled:

```sh
$ sudo ufw show added
Added user rules (see 'ufw status' for running firewall):
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443
```

Enable firewall:

```sh
$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```

The firewall is now active.

# Operations

## Status

Run the `sudo ufw status verbose` command to see the rules that are set:

```sh
$ sudo ufw status verbose
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
80/tcp                     ALLOW IN    Anywhere                  
443                        ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
80/tcp (v6)                ALLOW IN    Anywhere (v6)             
443 (v6)                   ALLOW IN    Anywhere (v6)
```

## Log

```sh
$ sudo journalctl | grep UFW
```

Even if the server is shortly installed, we see a lot of blocked attempts!

* An `UFW BLOCK` is simply indicating that packet was blocked.
* `SRC=1.164.42.160` indicates the IP address of the packet source.
* `DPT=8728` ("MikroTik API port") indicates the destination port of the packet.

## Scan server ports with nmap

> WARNING: Do not scan a server that is not your own!

Let's check out our server how it is seen from outside.

Install `nmap` on your client system:

```sh
$ sudo apt install nmap
```

Now do a scan on your own server:

```sh
$ sudo nmap -sS -p1-65535 your_server
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-02 14:08 CET
Nmap scan report for your_server (your_servers_IP)
Host is up (0.075s latency).
rDNS record for your_servers_IP: ipyour_servers_IP.pbiaas.com
Not shown: 44715 filtered tcp ports (no-response), 20816 filtered tcp ports (admin-prohibited)
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   closed http
443/tcp  closed https
3544/tcp closed teredo

Nmap done: 1 IP address (1 host up) scanned in 194.15 seconds

```

The `teredo` port seems to be necessary for IPv6 to work [Wikipedia](https://en.wikipedia.org/wiki/Teredo_tunneling).
