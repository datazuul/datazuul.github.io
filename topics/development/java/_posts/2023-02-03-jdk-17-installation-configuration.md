---
layout: post
title: JDK 17 (Java Development Kit) installation and configuration
author: Ralf Eichinger
toc: true
---

Before you can start to develop Java applications or webapps you have to install the JDK (the Java SE Development Kit).

> The JDK is a development environment for building applications, applets, and components using the Java programming language.
> The JDK includes tools useful for developing and testing programs written in the Java programming language and running on the Java platform.

On most systems Java is preinstalled, but only the Runtime Environment (JRE). The Runtime Environment lacks developer tools like the Java compiler "javac".

To see if and which Java Runtime Environment (JRE) is installed, execute "java -version" on the command line. Example (Ubuntu 22.10 Linux):

```sh
$ java -version
Der Befehl 'java' wurde nicht gefunden, kann aber installiert werden mit:
sudo apt install openjdk-11-jre-headless  # version 11.0.17+8-1ubuntu2~22.04, or
sudo apt install default-jre              # version 2:1.11-72build2
sudo apt install openjdk-17-jre-headless  # version 17.0.5+8-2ubuntu1~22.04
sudo apt install openjdk-18-jre-headless  # version 18.0.2+9-2~22.04
sudo apt install openjdk-19-jre-headless  # version 19.0.1+10-1ubuntu1~22.04
sudo apt install openjdk-8-jre-headless   # version 8u352-ga-1~22.04
```

This tells us, that the OpenJDK JRE is not installed.

To see if and which Java Development Kit (JDK) is installed, execute "javac -version" on the command line. Example (Ubuntu 22.10 Linux):

```sh
$ javac -version
Der Befehl 'javac' wurde nicht gefunden, kann aber installiert werden mit:
sudo apt install openjdk-11-jdk-headless  # version 11.0.17+8-1ubuntu2~22.04, or
sudo apt install default-jdk              # version 2:1.11-72build2
sudo apt install openjdk-17-jdk-headless  # version 17.0.5+8-2ubuntu1~22.04
sudo apt install openjdk-18-jdk-headless  # version 18.0.2+9-2~22.04
sudo apt install openjdk-19-jdk-headless  # version 19.0.1+10-1ubuntu1~22.04
sudo apt install openjdk-8-jdk-headless   # version 8u352-ga-1~22.04
sudo apt install ecj                      # version 3.16.0-1
```

This tells us, that the OpenJDK is not installed.

# Specification

* Homepage: <https://openjdk.org/>
* Version: JDK 19.0.2
* Download: <https://jdk.java.net/19/>

# Installation

As we want to install JDK 17, which is available as Ubuntu package from standard repositories,
we just follow the above installation hints (without trailing "-headless"). But do not install a "headless" package,
otherwise GUI-applications will not work!

## Ubuntu 22.10

```sh
$ sudo apt install openjdk-17-jdk
...
```

# Test

## Ubuntu 22.10

Open a new terminal

```sh
$ which java
/usr/bin/java

$ java -version
openjdk version "17.0.5" 2022-10-18
OpenJDK Runtime Environment (build 17.0.5+8-Ubuntu-2ubuntu122.04)
OpenJDK 64-Bit Server VM (build 17.0.5+8-Ubuntu-2ubuntu122.04, mixed mode, sharing)
```
