---
layout: post
title: Eclipse IDE
author: Ralf Eichinger
toc: true
---

For a professional Java Development Environment you need an IDE (Integrated Development Environment):

> An IDE ... is a software application that provides comprehensive facilities to computer programmers for software development. An IDE normally consists of a source code editor, build automation tools and a debugger.

Eclipse, IntelliJ and NetBeans are the most used IDEs.

In this tutorial you will install Eclipse for JEE developers (version focused on webapp developers).

# Specification

* Homepage: <http://www.eclipse.org/>
* Download: <http://eclipse.org/downloads/>
* Version: Eclipse Kepler (4.3.1) SR1, Eclipse IDE for Java EE Developers
* File(s):
  * Windows: eclipse-jee-kepler-SR1-win32.zip
  * Linux: eclipse-jee-kepler-SR1-linux-gtk.tar.gz (245 MB)

# Installation

## Windows

* Unzip ZIP-file to an appropriate folder (C:\Program Files)
* Rename unpacked folder "eclipse" to "Eclipse_Kepler"
* Create a desktop shortcut or quick launch icon to the executable "eclipse.exe" in the unpacked folder

## Linux

It is recommended to install it not system wide (e.g. in "/usr/local/bin"), but per user in the user's home. Otherwise problems with some plugins may appear (e.g. after installing m2eclipse other plugins disappeared...):

```sh
$ cd /home/ralf/
$ mkdir Programs
$ cd Programs
$ tar xvfz /home/ralf/Downloads/eclipse-jee-kepler-SR1-linux-gtk.tar.gz
$ mv eclipse/ Eclipse_Kepler
```

# Configuration

## Startup Parameters

Change start parameters in `C:\Program Files\Eclipse_Kepler\eclipse.ini` or `/home/<user>/Programs/Eclipse_Kepler/eclipse.ini`.

Note: Xmx and XX params must be appropriate to your available memory. If Eclipse does not start, down tune them.

### Windows

```sh
-showsplash
org.eclipse.platform
--launcher.XXMaxPermSize
256M
-vm
C:\Program Files\Java\jdk1.6.0_13\bin\javaw.exe
-framework
plugins\org.eclipse.osgi_3.4.3.R34x_v20081215-1030.jar
-vmargs
-Dosgi.requiredJavaVersion=1.5
-Xmx700m
-XX:MaxPermSize=690M
```

### Linux

```sh
-startup
plugins/org.eclipse.equinox.launcher_1.0.201.R35x_v20090715.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.gtk.linux.x86_1.0.200.v20090520
-product
org.eclipse.epp.package.jee.product
-showsplash
org.eclipse.platform
--launcher.XXMaxPermSize
256m
-vmargs
-Dosgi.requiredJavaVersion=1.5
-XX:MaxPermSize=690m
-Xms40m
-Xmx700m
```

## Workspaces

* Start Eclipse
* In Workspace Launcher point the "Workspace:"\
  field to a new Workspace-folder like "C:\Documents and Settings\<username>\My Documents\DEV\<companyname>" (Windows) or `/home/<username>/DEV/<companyname>` (Linux).
  Better would be a very short path for the workspace directory (to avoid later problems if your project has a very deep folder hierarchy/packages), e.g. "D:\WORKSPACES\<companyname>" (Windows).
  Don't check "Use this as default..." (so it will be easier to switch between different workspaces later)
* Close "Welcome" tab.

## Basic Preference Settings

```
Window - Preferences:
  General: Show heap status
  General - Editors - Text Editors: Show line numbers
  General - Editors - Text Editors - Spelling - Dictionaries: Encoding: UTF-8
  General - Network Connections: Manual proxy configuration
    HTTP proxy: <company_proxy_address>, Port: <company_proxy_port>
    Use this proxy server for SSL
    No proxy for: add internal addresses
    Enable proxy authentication: <username>/<password>
  General - Workspace: Text file encoding: Other: UTF-8
    Java - Code Style - Formatter - Java Conventions [built-in] \
      -> Use Sun Java Code Conventions 
```

## Proxy Settings

Problem: Eclipse 3.5 (Galileo) has problems using authenticated NTML proxy.\
Solution: The Apache httpclient implementation should be disabled because it doesn’t work well with NTMLv2 proxies. For NTLMv2 Proxies, that require user name and password for access the workaround is to Disable the ECF httpclient provider. Provide the NTLMv2 proxy authentication info (proxyhost, domain, username, and password)

Edit eclipse.ini and append the following properties:

```
-Dorg.eclipse.ecf.provider.filetransfer.excludeContributors=\
   org.eclipse.ecf.provider.filetransfer.httpclient
-Dhttp.proxyPort=8080
-Dhttp.proxyHost=myproxy
-Dhttp.proxyUser=mydomain\myusername
-Dhttp.proxyPassword=mypassword
-Dhttp.nonProxyHosts=localhost|127.0.0.1
```
