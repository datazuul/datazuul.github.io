---
layout: post
title: Apache Tomcat - Installation, Configuration and Usage (on localhost)
author: Ralf Eichinger
toc: true
---

First you have to choose which version you want to install, see <http://tomcat.apache.org/whichversion.html>:

> "Apache Tomcat 7.x is the current focus of development. It builds upon the improvements made in Tomcat 6.0.x and implements the Servlet 3.0, JSP 2.2 and EL 2.2 specifications. In addition to that, it includes the following improvements:
>
> * Web application memory leak detection and prevention
> * Improved security for the Manager and Host Manager applications
> * Generic CSRF protection
> * Support for including external content directly in a web application
> * Refactoring (connectors, lifecycle) and lots of internal code clean-up"

We choose to install for our local development environment the newest version 7.

# Specification

* Homepage: http://tomcat.apache.org/
* Version: 7.0.47
* File(s): apache-tomcat-7.0.47.tar.gz (8,3 MB) or the ZIP-file for Windows

# Installation

Unzip ZIP-file to an appropriate folder.

## Windows

Unzip to "C:\Program Files", new folder is “apache-tomcat-7.0.47”.

## Linux

Unzip to `/home/<username>/Programs`, new folder is “apache-tomcat-7.0.47”.

# Configuration

Setting CATALINA_HOME environment variable

## Windows

```
Start – Settings – Control Panel – System – Advanced – Environment Variables
System variables – New: CATALINA _HOME = C:\Program Files\apache-tomcat-7.0.47
```

## Linux

```sh
$ nano ~/.bashrc
...
export CATALINA_HOME="/home/<username>/Programs/apache-tomcat-7.0.47"
$ source ~/.bashrc
$ echo $CATALINA_HOME
~/Programs/apache-tomcat-7.0.47
```

# Test

## Windows

* Start Tomcat
  Open a new command prompt window, execute start script:

  ```sh
  C:\>"%CATALINA_HOME%"\bin\startup.bat
  Using CATALINA_BASE: C:\Program Files\apache-tomcat-7.0.47
  Using CATALINA_HOME: C:\Program Files\apache-tomcat-7.0.47
  Using CATALINA_TMPDIR: C:\Program Files\apache-tomcat-7.0.47\temp
  Using JRE_HOME: C:\Program Files\Java\jdk1.6.0_13
  C:\>
  ```

* Test running server: Go to URL http://localhost:8080/ in your browser
  You should see the welcome page of Tomcat

* Stop Tomcat

  ```sh
  C:\>"%CATALINA_HOME%"\bin\shutdown.bat
  Using CATALINA_BASE: C:\Program Files\apache-tomcat-7.0.47
  Using CATALINA_HOME: C:\Program Files\apache-tomcat-7.0.47
  Using CATALINA_TMPDIR: C:\Program Files\apache-tomcat-7.0.47\temp
  Using JRE_HOME: C:\Program Files\Java\jdk1.6.0_13
  C:\>
  ```

## Linux

* Start Tomcat
  Open a new terminal window, execute start script:

  ```sh
  $ $CATALINA_HOME/bin/startup.sh
  Using CATALINA_BASE:   /home/ralf/Programs/apache-tomcat-7.0.47
  Using CATALINA_HOME:   /home/ralf/Programs/apache-tomcat-7.0.47
  Using CATALINA_TMPDIR: /home/ralf/Programs/apache-tomcat-7.0.47/temp
  Using JRE_HOME:        /opt/jdk1.7.0_45
  Using CLASSPATH:       /home/ralf/Programs/apache-tomcat-7.0.47/\
    bin/bootstrap.jar:/home/ralf/Programs/apache-tomcat-7.0.47/\
    bin/tomcat-juli.jar
  $
  ```

* Test running server: Go to URL http://localhost:8080/ in your browser
  You should see the welcome page of Tomcat

* Stop Tomcat

  ```sh
  $ $CATALINA_HOME/bin/shutdown.sh
  Using CATALINA_BASE:   /home/ralf/Programs/apache-tomcat-7.0.47
  Using CATALINA_HOME:   /home/ralf/Programs/apache-tomcat-7.0.47
  Using CATALINA_TMPDIR: /home/ralf/Programs/apache-tomcat-7.0.47/temp
  Using JRE_HOME:        /opt/jdk1.7.0_45
  Using CLASSPATH:       /home/ralf/Programs/apache-tomcat-7.0.47/\
    bin/bootstrap.jar:/home/ralf/Programs/apache-tomcat-7.0.47/\
    bin/tomcat-juli.jar
  ```
