---
layout: post
title: Apache Solr 9 - Installation, Configuration and Usage (on localhost)
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://solr.apache.org/>
* Download: <https://solr.apache.org/downloads.html>
* Version 9.0.0
* File: solr-9.0.0.tgz (214 MB)

# Prerequisites

See <https://solr.apache.org/guide/solr/latest/deployment-guide/system-requirements.html>.

Make sure Java version 11 or higher is installed:

```
$ java -version
openjdk version "11.0.16" 2022-07-19
OpenJDK Runtime Environment (build 11.0.16+8-post-Debian-1deb11u1)
OpenJDK 64-Bit Server VM (build 11.0.16+8-post-Debian-1deb11u1, mixed mode, sharing)
```

# Installation

Installation is described in <https://solr.apache.org/guide/solr/latest/deployment-guide/installing-solr.html>:

* Download binary package from <https://solr.apache.org/downloads.html>, e.g. <https://www.apache.org/dyn/closer.lua/solr/solr/9.0.0/solr-9.0.0.tgz?action=download>
* Unpack it in target directory:

```
$ cd /opt/
$ sudo tar xvfz ~/Downloads/solr-9.0.0.tgz
$ ls -al solr-9.0.0/
insgesamt 1132
drwxr-xr-x 10 root root    4096  5. Mai 01:00 .
drwxr-xr-x 15 root root    4096 15. Jun 14:04 ..
drwxr-xr-x  3 root root    4096 25. Apr 22:37 bin
-rw-r--r--  1 root root 1010825  4. Mai 23:49 CHANGES.txt
drwxr-xr-x  3 root root    4096  5. Mai 01:00 docker
drwxr-xr-x  3 root root    4096  5. Mai 01:00 docs
drwxr-xr-x  4 root root    4096  5. Mai 01:00 example
drwxr-xr-x  2 root root   53248  4. Mai 23:49 licenses
-rw-r--r--  1 root root   12646 19. Jan 00:45 LICENSE.txt
drwxr-xr-x 16 root root    4096  5. Mai 01:00 modules
-rw-r--r--  1 root root   30460  4. Mai 23:49 NOTICE.txt
drwxr-xr-x  5 root root    4096  5. Mai 01:00 prometheus-exporter
-rw-r--r--  1 root root    7837 15. Feb 15:54 README.md
drwxr-xr-x 10 root root    4096  5. Mai 01:00 server
```

The directory `/opt/solr-9.0.0` often is referred to as `$SOLR_INSTALL`.

Create an user for Solr (with home directory `/home/solr` and login shell `bash`) and change file owner:

```
$ sudo useradd -m solr -s /bin/bash
$ ls -al /home/solr/
insgesamt 20
drwxr-xr-x 2 solr solr 4096 15. Jun 14:22 .
drwxr-xr-x 4 root root 4096 15. Jun 14:22 ..
-rw-r--r-- 1 solr solr  220 15. Jun 14:22 .bash_logout
-rw-r--r-- 1 solr solr 3526 15. Jun 14:22 .bashrc
-rw-r--r-- 1 solr solr  807 15. Jun 14:22 .profile
$ sudo chown -R solr:solr /opt/solr-9.0.0/
$ ls -al solr-9.0.0/
insgesamt 1132
drwxr-xr-x 10 solr solr    4096  5. Mai 01:00 .
drwxr-xr-x 15 root root    4096 15. Jun 14:04 ..
drwxr-xr-x  3 solr solr    4096 25. Apr 22:37 bin
-rw-r--r--  1 solr solr 1010825  4. Mai 23:49 CHANGES.txt
drwxr-xr-x  3 solr solr    4096  5. Mai 01:00 docker
drwxr-xr-x  3 solr solr    4096  5. Mai 01:00 docs
drwxr-xr-x  4 solr solr    4096  5. Mai 01:00 example
drwxr-xr-x  2 solr solr   53248  4. Mai 23:49 licenses
-rw-r--r--  1 solr solr   12646 19. Jan 00:45 LICENSE.txt
drwxr-xr-x 16 solr solr    4096  5. Mai 01:00 modules
-rw-r--r--  1 solr solr   30460  4. Mai 23:49 NOTICE.txt
drwxr-xr-x  5 solr solr    4096  5. Mai 01:00 prometheus-exporter
-rw-r--r--  1 solr solr    7837 15. Feb 15:54 README.md
drwxr-xr-x 10 solr solr    4096  5. Mai 01:00 server
```

# Usage

## Start/Stop Solr

Start Solr (with user `solr`):

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr start
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh
Waiting up to 180 seconds to see Solr running on port 8983 [\]  
Started Solr server on port 8983 (pid=17197). Happy searching!
```

Browse <http://localhost:8983/solr/>

![start screen solr](/assets/topics/server/apache-solr/Apache_Solr_Admin-9.0.0.png)

Stop Solr (with user `solr`):

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr stop -all
Sending stop command to Solr running on port 8983 ... waiting up to 180 seconds to allow Jetty process 17197 to stop gracefully.
```

# Further reading

After successful installation you should continue with our tutorial on indexing and searching example data ["Apache Solr 9 - Tutorial 1 - Index and search 'techproducts' example data"]({% post_url topics/server/apache-solr/2022-09-13-apache-solr-9-tutorial1-techproducts %}). Happy searching!