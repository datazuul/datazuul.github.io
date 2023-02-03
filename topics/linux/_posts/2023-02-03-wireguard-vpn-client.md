---
layout: post
title: Wireguard VPN-Client (Linux)
author: Ralf Eichinger
toc: true
---

This post describes how to install a wireguard VPN-client under Linux.

# Specification

* Homepage: <https://www.wireguard.com/>
* Current Version: v1.0.20210914

# Installation

## Ubuntu 22.10

```sh
$ sudo apt install wireguard
...
$ wg --version
wireguard-tools v1.0.20210914 - https://git.zx2c4.com/wireguard-tools/
```

# Configuration

Create Public/Private-Key pair:

```sh
$ wg genkey | tee /dev/tty | wg pubkey
fjhdskjfhdsafdsfkjhsadkfhkkfhds
zrewirziwzruewruiwzreiwzriuewzr
```

(The above output is just dummy placeholders.)
The first line is the private key.
The second line is the public key.

Do not save output anywhere!

* Send public key to Wireguard-Server-Admins. Wait for answer (telling you your IP-address)
* Add private key and your VPN ip address into configuration file `etc/wireguard/wg0.conf`:

```txt
[Interface]
PrivateKey = ** your private key from above output **
Address = ** your vpn ip address **
MTU = 1364

[Peer]
PublicKey = ** public key of wireguard gateway server - NOT above public key!!! **
AllowedIPs = ** comma separated list of IPs to be routed over VPN **
Endpoint = ** VPN gateway server address (and port) **
PersistentKeepalive = 25
```

Secure config file (only root readable/writable):

```sh
$ sudo chmod -R 700 /etc/wireguard
```

# Usage

## Manually start/stop

* Start: `sudo wg-quick up wg0`
* Stop: `sudo wg-quick down wg0`

## Automatically

```sh
$ sudo systemctl enable wg-quick@wg0
```

* Start: `sudo systemctl start wg-quick@wg0`
* Stop: `sudo systemctl stop wg-quick@wg0`