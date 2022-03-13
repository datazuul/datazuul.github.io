---
layout: post
title: Linux Networking Tools
author: Ralf Eichinger
toc: true
---

# nmap

## Installation

```sh
$ sudo apt install nmap
```

## Find free IP-address in network

If you are in a network with fixed IP-Addresses (like 192.168.173.0/24), you can scan the network to see which hosts are online/offline:


```sh
$ nmap -v -sn 192.168.173.0/24 

Starting Nmap 6.40 ( http://nmap.org ) at 2014-08-21 10:18 CEST 
Initiating Ping Scan at 10:18 
Scanning 256 hosts [2 ports/host] 
Completed Ping Scan at 10:18, 6.76s elapsed (256 total hosts) 
Initiating Parallel DNS resolution of 256 hosts. At 10:18 
Completed Parallel DNS resolution of 256 hosts. at 10:18, 0.01s elapsed 
Nmap scan report for 192.168.173.0 [host down] 
Nmap scan report for 192.168.173.1 
Host is up (0.0010s latency). 
Nmap scan report for 192.168.173.2 [host down] 
Nmap scan report for 192.168.173.3 [host down] 
Nmap scan report for 192.168.173.4 [host down] 
Nmap scan report for 192.168.173.5 [host down] 
…
Nmap scan report for 192.168.173.12 
Host is up (0.00058s latency). 
Nmap scan report for 192.168.173.13 
Host is up (0.0017s latency). 
Nmap scan report for 192.168.173.14 [host down] 
Nmap scan report for 192.168.173.15 [host down] 
…
Nmap scan report for 192.168.173.47 
Host is up (0.00058s latency). 
Nmap scan report for 192.168.173.48 [host down] 
Nmap scan report for 192.168.173.49 [host down] 
Nmap scan report for 192.168.173.50 [host down] 
…
Nmap scan report for 192.168.173.254 [host down] 
Nmap scan report for 192.168.173.255 [host down] 
Read data files from: /usr/bin/../share/nmap 

Nmap done: 256 IP addresses (39 hosts up) scanned in 6.77 seconds
```
