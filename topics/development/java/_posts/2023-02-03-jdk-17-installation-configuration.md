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

To see if and which Java environment is installed, execute "java -version" on the command line. Example (Linux Mint):

```sh
# java -version
java version "1.7.0_25"
OpenJDK Runtime Environment (IcedTea 2.3.10) (7u25-2.3.10-1ubuntu0.13.04.2)
OpenJDK Server VM (build 23.7-b01, mixed mode)
```

This tells us, that the OpenJDK Runtime Environment is installed. OpenJDK is an open source implementation of Java. The original Java Company was Sun Microsystems and now Oracle. Oracle offers the market leader JDK, which we are going to install now.

# Specification

* Homepage: http://www.oracle.com/technetwork/java/index.html
* Version: Java SE 7 Update 45
* Download: http://www.oracle.com/technetwork/java/javase/downloads/index.html
* File(s):
  * for Windows: jdk-7u45-windows-i586.exe (32-bit, 124 MB)
  * for Linux: jdk-7u45-linux-i586.tar.gz (124 MB)

# Installation

The following installation guide may differ in JDK versions between different systems, but even if version is outdated the installation process should be still the same.

## Windows

* Execute EXE-File
* Wizard:
  * Accept License Agreement
  * Install all components (Java DB not needed, but is only 25 MB...)
  * Target directories (defaults, don't change anything), e.g.:
    ```sh
    Development Tools: C:\Program Files\Java\jdk1.6.0_13\
    Demos and Samples: C:\Program Files\Java\jdk1.6.0_13\
    Source Code: C:\Program Files\Java\jdk1.6.0_13\
    Public JRE: C:\Program Files\Java\jre6\
    Java DB: C:\Program Files\Sun\JavaDB\
    ```
* After installation a popup appears, which wants you to register. You don't need to do this.

## Linux

### Ubuntu

#### SUN JDK 6

* Execute Synaptic package manager
* Install "sun-java6-jdk"
* Agree to license shown in dialog

#### Downloaded Oracle JDK

Unpack the downloaded TAR.GZ-package into a suitable directory (as root):

```sh
# mkdir -p /usr/lib/jvm
# cd /usr/lib/jvm/
# tar xvfz /home/ralf/Downloads/jdk-7u45-linux-i586.tar.gz
# ls -al
total 52
drwxr-xr-x   6 root root  4096 Okt 30 17:08 .
drwxr-xr-x 180 root root 28672 Okt 28 22:45 ..
lrwxrwxrwx   1 root root    23 Okt 28 22:15 default-java\
  -> java-1.7.0-openjdk-i386
lrwxrwxrwx   1 root root    19 Okt 28 22:15 java-1.7.0-openjdk-i386\
  -> java-7-openjdk-i386
-rw-r--r--   1 root root  2396 Jul  3 21:59 .java-1.7.0-openjdk-i386.jinfo
drwxr-xr-x   5 root root  4096 Apr 25  2013 java-6-openjdk-i386
drwxr-xr-x   3 root root  4096 Apr 25  2013 java-7-openjdk-common
drwxr-xr-x   5 root root  4096 Apr 25  2013 java-7-openjdk-i386
drwxr-xr-x   8 uucp  143  4096 Okt  8 15:07 jdk1.7.0_45
```

Install the executables as default (with very high priority, e.g. 2000) (as root):

```sh
# sudo update-alternatives --install "/usr/bin/java" "java"\
  "/usr/lib/jvm/jdk1.7.0_45/bin/java" 2000
update-alternatives: using /usr/lib/jvm/jdk1.7.0_45/bin/java\
  to provide /usr/bin/java (java) in auto mode
# sudo update-alternatives --install "/usr/bin/javac" "javac"\
  "/usr/lib/jvm/jdk1.7.0_45/bin/javac" 2000
update-alternatives: using /usr/lib/jvm/jdk1.7.0_45/bin/javac\
  to provide /usr/bin/javac (javac) in auto mode
# sudo update-alternatives --install "/usr/bin/javaws" "javaws"\
  "/usr/lib/jvm/jdk1.7.0_45/bin/javaws" 2000
update-alternatives: using /usr/lib/jvm/jdk1.7.0_45/bin/javaws\
  to provide /usr/bin/javaws (javaws) in auto mode
# sudo update-alternatives --install "/usr/bin/jar" "jar"\
  "/usr/lib/jvm/jdk1.7.0_45/bin/jar" 2000
update-alternatives: using /usr/lib/jvm/jdk1.7.0_45/bin/jar\
  to provide /usr/bin/jar (jar) in auto mode
```

### Debian

#### SUN JDK 6

```sh
(# apt-cache search jdk –n)
(# apt-get install sun-java6-jdk –s)
# apt-get install sun-java6-jdk
```

### Slackware based Distros

```sh
$ su -
Password: 
# cd /opt/
# ls -al
total 4
drwxr-xr-x  2 root root    6 Jun 10  2007 ./
drwxr-xr-x 20 root root 4096 Oct 31 22:07 ../
# tar xvfz /home/ralf/Downloads/jdk-7u45-linux-i586.tar.gz
...
# ls -al
total 8
drwxr-xr-x  3 root root   24 Oct 31 22:11 ./
drwxr-xr-x 20 root root 4096 Oct 31 22:07 ../
drwxr-xr-x  8 uucp  143 4096 Oct  8 15:07 jdk1.7.0_45/
```

# Configuration

## Add JAVA_HOME environment variable

###  Windows

* Start - Settings - Control Panel - System - Advanced - Environment Variables
* System variables - New: JAVA_HOME = C:\Program Files\Java\jdk1.6.0_13

### Ubuntu Linux

```sh
# nano /etc/environment
...
JAVA_HOME="/usr/lib/jvm/java-6-sun/"
...
# source /etc/environment
```

### Linux Mint

```sh
# nano /etc/environment
...
JAVA_HOME="/usr/lib/jvm/jdk1.7.0_45"
...
# source /etc/environment
```

### Slackware based Distros

```sh
# vi /etc/profile.d/jdk.csh
#!/bin/csh

setenv JAVA_HOME /opt/jdk1.7.0_45
setenv MANPATH ${MANPATH}:${JAVA_HOME}/man
setenv PATH ${PATH}:${JAVA_HOME}/bin

# vi /etc/profile.d/jdk.sh
#!/bin/sh

export JAVA_HOME=/opt/jdk1.7.0_45
export MANPATH="$MANPATH:$JAVA_HOME/man"
export PATH="$PATH:$JAVA_HOME/bin"

# chmod +x /etc/profile.d/jdk.sh
# chmod +x /etc/profile.d/jdk.csh
# chmod -x /etc/profile.d/openjre.sh
# chmod -x /etc/profile.d/openjre.csh
```

# Test

## Windows

Open a new command prompt window (Start - Programs - Accessoires - Command Prompt)

```sh
C:\>java -version
java version "1.6.0_13"
Java(TM) SE Runtime Environment (build 1.6.0_13-b03)
Java HotSpot(TM) Client VM (build 11.3-b02, mixed mode, sharing)
C:\>echo %JAVA_HOME%
C:\Program Files\Java\jdk1.6.0_13
```

## Ubuntu

Open a new terminal

```sh
$ java -version
java version "1.6.0_16"
Java(TM) SE Runtime Environment (build 1.6.0_16-b01)
Java HotSpot(TM) Server VM (build 14.2-b01, mixed mode)
$ echo $JAVA_HOME
/usr/lib/jvm/java-6-sun/
```

## Other Distro

```sh
# java -version
java version "1.7.0_45"
Java(TM) SE Runtime Environment (build 1.7.0_45-b18)
Java HotSpot(TM) Server VM (build 24.45-b08, mixed mode)
$ echo $JAVA_HOME
/usr/lib/jvm/jdk1.7.0_45
```

# Troubleshooting

## Download timeout because of proxy

Problem: Timeouts caused by proxy (download takes too long).\
Solution Debian Linux:

* Download "sun-java6"-packages manually onto your desktop system with browser (e.g. from http://ftp.hosteurope.de/pub/linux/debian/)
* Copy DEB-packages to server
* Install the packages using "dpkg –i":
  ```sh
  # dpkg –i sun-java6-bin_6-13-1_i386.deb
  # dpkg –i sun-java6-jdk_6-13-1_i386.deb
  # dpkg –i sun-java6-jre_6-13-1_all.deb
  ```
