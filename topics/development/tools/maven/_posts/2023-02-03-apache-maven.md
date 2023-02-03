---
layout: post
title: Apache Maven 3.6.3
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://maven.apache.org/>
* Current version: 3.8.7
* Download: <https://maven.apache.org/download.cgi>

# Installation

## Ubuntu 22.10

We install the default provided version 3.6.3 of Ubuntu 22.10.


```sh
$ mvn
Der Befehl 'mvn' wurde nicht gefunden, kann aber installiert werden mit:
sudo apt install maven
$ sudo apt install maven
...
maven (3.6.3-5) wird eingerichtet ...
update-alternatives: /usr/share/maven/bin/mvn wird verwendet, um /usr/bin/mvn (mvn) im automatischen Modus bereitzustellen
```

# Test

```sh
$ mvn --version
Apache Maven 3.6.3
Maven home: /usr/share/maven
Java version: 17.0.5, vendor: Private Build, runtime: /usr/lib/jvm/java-17-openjdk-amd64
Default locale: de_DE, platform encoding: UTF-8
OS name: "linux", version: "5.15.0-58-generic", arch: "amd64", family: "unix"
```
