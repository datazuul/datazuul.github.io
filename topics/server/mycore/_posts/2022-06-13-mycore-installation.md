---
layout: post
title: MyCoRe - Installation, Configuration and Usage (on localhost)
author: Ralf Eichinger
toc: true
---

MyCoRe ("My Content Repository") is a framework for presentation and management of digital content.

# Specification

* Homepage: <https://www.mycore.de/>
* Sourcecode: <https://github.com/MyCoRe-Org>
* Version: LTS 2021.06
* Documentation: <https://www.mycore.de/documentation/migrate/migrate_mcr2021_06/>

# Installation

MyCoRe is based on

* Java 11 (OpenJDK)
* Apache Tomcat 9.0 or Jetty 9.4.18 (alternativ ein System mit Unterstützung von Servlet-3.1)
* Apache Solr 8.9.0 or higher
* a hibernate-compatible relational database, e.g. PostgreSQL 10 or higher, MySQL/Maria-DB 10 or higher, DB2; (for testing purpose the embedded database H2 is sufficient)

In the following we use this setup:

* Debian Linux 11 ("bullseye") (from Debian repo)
* OpenJDK 11.0.15 (from Debian repo)
* Apache Tomcat 9.0.64 from <https://tomcat.apache.org/download-90.cgi>
* Apache Solr 9.0.0 from <https://solr.apache.org/downloads.html>
* Postgres 12.11 (from Debian repo)

## OpenJDK

```
$ sudo apt-get update
$ sudo apt-get install openjdk-11-jdk
$ java -version
openjdk version "11.0.15" 2022-04-19
OpenJDK Runtime Environment (build 11.0.15+10-post-Debian-1deb11u1)
OpenJDK 64-Bit Server VM (build 11.0.15+10-post-Debian-1deb11u1, mixed mode, sharing)
```

## Apache Tomcat

* Homepage: <https://tomcat.apache.org/>
* Download: <https://tomcat.apache.org/download-90.cgi>
* Version 9.0.64
* File: apache-tomcat-9.0.64.tar.gz (11 MB)

Installation is described in <https://tomcat.apache.org/tomcat-9.0-doc/RUNNING.txt>:

* Unpack binary download into target directory:

```
$ cd /opt/
$ sudo tar xfvz ~/Downloads/apache-tomcat-9.0.64.tar.gz
$ cd apache-tomcat-9.0.64/
$ ls -al
insgesamt 156
drwxr-xr-x 9 root root  4096 14. Jun 11:35 .
drwxr-xr-x 3 root root  4096 14. Jun 11:35 ..
drwxr-x--- 2 root root  4096 14. Jun 11:35 bin
-rw-r----- 1 root root 18986  2. Jun 21:08 BUILDING.txt
drwx------ 2 root root  4096  2. Jun 21:08 conf
-rw-r----- 1 root root  6210  2. Jun 21:08 CONTRIBUTING.md
drwxr-x--- 2 root root  4096 14. Jun 11:35 lib
-rw-r----- 1 root root 57092  2. Jun 21:08 LICENSE
drwxr-x--- 2 root root  4096  2. Jun 21:08 logs
-rw-r----- 1 root root  2333  2. Jun 21:08 NOTICE
-rw-r----- 1 root root  3398  2. Jun 21:08 README.md
-rw-r----- 1 root root  6901  2. Jun 21:08 RELEASE-NOTES
-rw-r----- 1 root root 16505  2. Jun 21:08 RUNNING.txt
drwxr-x--- 2 root root  4096 14. Jun 11:35 temp
drwxr-x--- 7 root root  4096  2. Jun 21:08 webapps
drwxr-x--- 2 root root  4096  2. Jun 21:08 work
```

* Create an user for Tomcat and change file owner

```
$ sudo useradd -m tomcat
$ ls -al /home/tomcat/
insgesamt 20
drwxr-xr-x 2 tomcat tomcat 4096 19. Jun 11:57 .
drwxr-xr-x 5 root   root   4096 19. Jun 11:57 ..
-rw-r--r-- 1 tomcat tomcat  220 18. Apr 2019  .bash_logout
-rw-r--r-- 1 tomcat tomcat 3526 18. Apr 2019  .bashrc
-rw-r--r-- 1 tomcat tomcat  807 18. Apr 2019  .profile
$ sudo chown -R tomcat:tomcat /opt/apache-tomcat-9.0.64/
```

* Configure Port

If the default port `8080` is already in use, configure a different port for Tomcat, e.g. `8081`:

```
$ sudo nano /opt/apache-tomcat-9.0.64/conf/server.xml
...
<Service name="Catalina">
  ...
  <Connector port="8081" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

* Configure Environment Variables

"Tomcat is a Java application and does not use environment variables directly.
Environment variables are used by the Tomcat startup scripts. The scripts use
the environment variables to prepare the command that starts Tomcat. ...
The `CATALINA_HOME` environment variable should be set to the location of the
root directory of the "binary" distribution of Tomcat. ...
Set `JRE_HOME` or `JAVA_HOME` (required) ... The `JRE_HOME` variable is used to specify location of a JRE. The `JAVA_HOME`
variable is used to specify location of a JDK. ... Using `JAVA_HOME` provides access to certain additional startup options that
are not allowed when `JRE_HOME` is used. ... If both `JRE_HOME` and `JAVA_HOME` are specified, `JRE_HOME` is used.
...
The recommended place to specify these variables is a "`setenv`" script. See
below.
...
The script is placed either into `CATALINA_BASE/bin` or into `CATALINA_HOME/bin` directory and is named
`setenv.bat` (on Windows) or `setenv.sh` (on Linux). The file has to be
readable."

```
$ which javac
/usr/lib/jvm/default-java/bin/javac
$ cd /opt/apache-tomcat-9.0.64
$ sudo nano bin/setenv.sh
#!/bin/sh  

JAVA_HOME=/usr/lib/jvm/default-java
CATALINA_HOME=/opt/apache-tomcat-9.0.64
$
```

* Startup Tomcat

```
$ sudo -u tomcat /opt/apache-tomcat-9.0.64/bin/catalina.sh start
```

Browse `http://localhost:8081/`:

![start screen tomcat](/assets/topics/server/mycore/Apache_Tomcat_9.0.64.png)

Tomcat is in process list:

```
$ ps -ef | grep tomcat
tomcat     10185       1 16 11:59 pts/1    00:00:05 /usr/lib/jvm/default-java/bin/java \
-Djava.util.logging.config.file=/opt/apache-tomcat-9.0.64/conf/logging.properties \
-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager \
-Djdk.tls.ephemeralDHKeySize=2048 \
-Djava.protocol.handler.pkgs=org.apache.catalina.webresources \
-Dorg.apache.catalina.security.SecurityListener.UMASK=0027 \
-Dignore.endorsed.dirs= -classpath /opt/apache-tomcat-9.0.64/bin/bootstrap.jar:/opt/apache-tomcat-9.0.64/bin/tomcat-juli.jar \
-Dcatalina.base=/opt/apache-tomcat-9.0.64 \
-Dcatalina.home=/opt/apache-tomcat-9.0.64 \
-Djava.io.tmpdir=/opt/apache-tomcat-9.0.64/temp org.apache.catalina.startup.Bootstrap start
```

* Shutdown Tomcat

```
$ sudo -u tomcat /opt/apache-tomcat-9.0.64/bin/catalina.sh stop
```

## Apache Solr

* Homepage: <https://solr.apache.org/>
* Download: <https://solr.apache.org/downloads.html>
* Version 9.0.0
* File: solr-9.0.0.tgz (214 MB)

See <https://www.mycore.de/documentation/getting_started/gs_solr8/>.

### Installation

Installation is described in <https://solr.apache.org/guide/solr/latest/deployment-guide/installing-solr.html>:

* Download binary package <https://www.apache.org/dyn/closer.lua/solr/solr/9.0.0/solr-9.0.0.tgz?action=download>
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

Create an user for Solr and change file owner:

```
$ sudo useradd -m solr
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

### Start/Stop Solr

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

![start screen solr](/assets/topics/server/mycore/Apache_Solr_Admin.png)

Stop Solr (with user `solr`):

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr stop
Sending stop command to Solr running on port 8983 ... waiting up to 180 seconds to allow Jetty process 17197 to stop gracefully.
```

## Postgres

* Homepage: <https://www.postgresql.org/>
* Download: <https://www.postgresql.org/download/>
* Version 12.11

See <https://www.mycore.de/documentation/getting_started/gs_requirements/> and <https://www.mycore.de/documentation/getting_started/gs_jpa/>.

### Installation

```
$ sudo apt-cache search postgresql-12
...
postgresql-12 - The World's Most Advanced Open Source Relational Database
...
$ sudo apt install postgresql-12
```

## bibutils

Format conversion util needed by MIR.

* Homepage: <https://sourceforge.net/projects/bibutils/>
* Download: <https://sourceforge.net/projects/bibutils/files/latest/download>
* Sourcecode: <https://sourceforge.net/projects/bibutils/files/>
* Version: 6.10

Install Debian package:

```
$ sudo apt-cache search bibutils
...
bibutils - Konvertiert zwischen verschiedenen bibliografischen Datenformaten
...
$ sudo apt install bibutils
...
bibutils (6.10-2) wird eingerichtet ...
...
$ bib2xml -v
bib2xml, bibutils suite version 6.10 date 2020-03-23
$ which bib2xml
/usr/bin/bib2xml
```

## MIR Webanwendung

* Homepage: <https://www.mycore.de/documentation/apps/mir/mir_install/>
* Download: <https://www.mycore.de/site/download/dl_mir/mir_release/>
* Sourcecode: <https://github.com/MyCoRe-Org/mir>
* Demo: <https://www.mycore.de/mir/content/index.xml>
* Version: 2021.06 (LTS)

There is a LTS version in the Download section and a current version of the snapshot in development under <https://oss.sonatype.org/service/local/artifact/maven/content?r=snapshots&g=org.mycore.mir&a=mir-webapp&v=LATEST&e=war>.
To be on the safe side, we install (for now) the stable LTS version.

### Solr Configuration

See <https://www.mycore.de/documentation/search/search_solr_use/> and <https://www.mycore.de/documentation/getting_started/gs_solr8/>.

It makes sense if you don't create the data directory with your own cores within the Solr distribution. The same applies to the log files.
Therefore we create a `cores` and a `logs` directory under the home directory `/home/solr` of the user `solr`:

```
$ sudo -u solr mkdir /home/solr/cores
$ sudo -u solr mkdir /home/solr/logs
$ ls -al /home/solr/
insgesamt 28
drwxr-xr-x 4 solr solr 4096 19. Jun 14:53 .
drwxr-xr-x 5 root root 4096 19. Jun 11:57 ..
-rw-r--r-- 1 solr solr  220 15. Jun 14:22 .bash_logout
-rw-r--r-- 1 solr solr 3526 15. Jun 14:22 .bashrc
drwxr-xr-x 2 solr solr 4096 19. Jun 14:53 cores
drwxr-xr-x 2 solr solr 4096 19. Jun 14:53 logs
-rw-r--r-- 1 solr solr  807 15. Jun 14:22 .profile
```

The directories have to be given during start of solr using command line parameters (see <https://solr.apache.org/guide/solr/latest/deployment-guide/solr-control-script-reference.html>).

Start Solr:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr start -s /home/solr/cores -Dsolr.log.dir=/home/solr/logs
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh
Waiting up to 180 seconds to see Solr running on port 8983 [\]  
Started Solr server on port 8983 (pid=18642). Happy searching!
```

The dashboard under <http://localhost:8983/solr/#/> is showing following args:

```
-Dsolr.log.dir=/home/solr/logs
-Dsolr.solr.home=/home/solr/cores
```

Copy configsets from MyCoRe repo:

```
$ cd /opt/solr-9.0.0/server/solr/configsets/
$ sudo -u solr git clone https://github.com/MyCoRe-Org/mycore_solr_configset_classification.git
$ sudo -u solr git clone https://github.com/MyCoRe-Org/mycore_solr_configset_main.git
$ ls -al
insgesamt 24
drwxr-xr-x 6 solr solr 4096 19. Jun 14:26 .
drwxr-xr-x 3 solr solr 4096  5. Mai 01:00 ..
drwxr-xr-x 3 solr solr 4096  5. Mai 01:00 _default
drwxr-xr-x 4 solr solr 4096 19. Jun 14:25 mycore_solr_configset_classification
drwxr-xr-x 4 solr solr 4096 19. Jun 14:26 mycore_solr_configset_main
drwxr-xr-x 3 solr solr 4096  5. Mai 01:00 sample_techproducts_configs
$
```

Create the Solr cores for MIR.

For creation of the cores the `XSLTResponseWriter` is needed.

WARNING: According to <https://solr.apache.org/guide/solr/latest/upgrade-notes/major-changes-in-solr-9.html>:
In Solr 9 the needed `solr.XSLTResponseWriter` has been moved for security reasons to scripting module.
We have to activate it explicitly, see <https://solr.apache.org/guide/solr/latest/configuration-guide/solr-modules.html>.
So we append `-Dsolr.modules=scripting` at start.

(Additionally add `analysis-extras` module as it is needed by MIR-Wizard).

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr stop -all
$ sudo -u solr /opt/solr-9.0.0/bin/solr start -s /home/solr/cores -Dsolr.log.dir=/home/solr/logs -Dsolr.modules=scripting,analysis-extras
```

For creation of the cores we have to adapt config sets not being ready for Solr 9.
Replace `solr.XSLTResponseWriter` with proper new class path `org.apache.solr.scripting.xslt.XSLTResponseWriter`
(see JAR-file `/opt/solr-9.0.0/modules/scripting/lib/solr-scripting-9.0.0.jar`):

```
$ sudo -u solr nano /opt/solr-9.0.0/server/solr/configsets/mycore_solr_configset_classification/conf/solrconfig.xml
...
<queryResponseWriter name="xslt" class="org.apache.solr.scripting.xslt.XSLTResponseWriter">
...
$ sudo -u solr nano /opt/solr-9.0.0/server/solr/configsets/mycore_solr_configset_main/conf/solrconfig.xml
...
<queryResponseWriter name="xslt" class="org.apache.solr.scripting.xslt.XSLTResponseWriter">
...
```

Create cores:

```
$ cd /opt/solr-9.0.0/bin
$ sudo -u solr ./solr create -d mycore_solr_configset_classification -c mir-class
Created new core 'mir-class'
$ sudo -u solr ./solr create -d mycore_solr_configset_main -c mir-main
Created new core 'mir-main'
```

INFO: If errors during creation, delete broken core with `sudo -u solr ./solr delete -c mir-class`

Browse <http://localhost:8983/solr/#/~cores/> to see the newly created cores.
They live under `/home/solr/cores/mir-class` and `/home/solr/cores/mir-main`.

### Postgresql Configuration

Create a databse `mir` for MIR using an user e.g. `mir`. Take note of given password for later configuration:

```
$ sudo su - postgres
$ psql
psql (12.11 (Debian 12.11-1.pgdg100+1))
Geben Sie »help« für Hilfe ein.

postgres=# CREATE DATABASE mir;
CREATE DATABASE
postgres=# CREATE USER mir WITH PASSWORD 'place_users_password_here';
CREATE ROLE
postgres=# GRANT ALL PRIVILEGES ON DATABASE "mir" to mir;
GRANT
postgres-# \q
postgres@t490:~$ exit
Abgemeldet
```

Install Postgresql JDBC driver package:

```
$ sudo apt-cache search jdbc
...
libpostgresql-jdbc-java - Java database (JDBC) driver for PostgreSQL
...
$ sudo apt install libpostgresql-jdbc-java
$ dpkg -L libpostgresql-jdbc-java
/.
/usr
/usr/share
/usr/share/doc
/usr/share/doc/libpostgresql-jdbc-java
/usr/share/doc/libpostgresql-jdbc-java/README.md.gz
/usr/share/doc/libpostgresql-jdbc-java/changelog.Debian.gz
/usr/share/doc/libpostgresql-jdbc-java/copyright
/usr/share/java
/usr/share/java/postgresql.jar
/usr/share/maven-repo
/usr/share/maven-repo/org
/usr/share/maven-repo/org/postgresql
/usr/share/maven-repo/org/postgresql/postgresql
/usr/share/maven-repo/org/postgresql/postgresql/42.3.5
/usr/share/maven-repo/org/postgresql/postgresql/42.3.5/postgresql-42.3.5.pom
/usr/share/maven-repo/org/postgresql/postgresql/debian
/usr/share/maven-repo/org/postgresql/postgresql/debian/postgresql-debian.pom
/usr/share/maven-repo/postgresql
/usr/share/maven-repo/postgresql/postgresql
/usr/share/maven-repo/postgresql/postgresql/debian
/usr/share/maven-repo/postgresql/postgresql/debian/postgresql-debian.pom
/usr/share/java/postgresql-42.3.5.jar
/usr/share/java/postgresql-jdbc3.jar
/usr/share/java/postgresql-jdbc4.jar
/usr/share/maven-repo/org/postgresql/postgresql/42.3.5/postgresql-42.3.5.jar
/usr/share/maven-repo/org/postgresql/postgresql/debian/postgresql-debian.jar
```

Copy JDBC-driver of Postgresql into `lib` directory of tomcat:

```
$ sudo -u tomcat cp /usr/share/java/postgresql.jar /opt/apache-tomcat-9.0.64/lib/
$ sudo -u tomcat ls -al /opt/apache-tomcat-9.0.64/lib/
insgesamt 10824
drwxr-x--- 2 tomcat tomcat    4096 19. Jun 17:07 .
drwxr-xr-x 9 tomcat tomcat    4096 14. Jun 11:35 ..
-rw-r----- 1 tomcat tomcat   13197  2. Jun 21:08 annotations-api.jar
-rw-r----- 1 tomcat tomcat   54063  2. Jun 21:08 catalina-ant.jar
-rw-r----- 1 tomcat tomcat  123877  2. Jun 21:08 catalina-ha.jar
-rw-r----- 1 tomcat tomcat 1724738  2. Jun 21:08 catalina.jar
-rw-r----- 1 tomcat tomcat   62797  2. Jun 21:08 catalina-ssi.jar
-rw-r----- 1 tomcat tomcat   78049  2. Jun 21:08 catalina-storeconfig.jar
-rw-r----- 1 tomcat tomcat  337039  2. Jun 21:08 catalina-tribes.jar
-rw-r----- 1 tomcat tomcat 3133846  2. Jun 21:08 ecj-4.20.jar
-rw-r----- 1 tomcat tomcat   88927  2. Jun 21:08 el-api.jar
-rw-r----- 1 tomcat tomcat  171510  2. Jun 21:08 jasper-el.jar
-rw-r----- 1 tomcat tomcat  567237  2. Jun 21:08 jasper.jar
-rw-r----- 1 tomcat tomcat   27125  2. Jun 21:08 jaspic-api.jar
-rw-r----- 1 tomcat tomcat   63554  2. Jun 21:08 jsp-api.jar
-rw-r--r-- 1 tomcat tomcat 1011786 19. Jun 17:07 postgresql.jar
-rw-r----- 1 tomcat tomcat  284303  2. Jun 21:08 servlet-api.jar
-rw-r----- 1 tomcat tomcat   11515  2. Jun 21:08 tomcat-api.jar
-rw-r----- 1 tomcat tomcat  924134  2. Jun 21:08 tomcat-coyote.jar
-rw-r----- 1 tomcat tomcat  342428  2. Jun 21:08 tomcat-dbcp.jar
-rw-r----- 1 tomcat tomcat   69607  2. Jun 21:08 tomcat-i18n-cs.jar
-rw-r----- 1 tomcat tomcat   77871  2. Jun 21:08 tomcat-i18n-de.jar
-rw-r----- 1 tomcat tomcat  105207  2. Jun 21:08 tomcat-i18n-es.jar
-rw-r----- 1 tomcat tomcat  167865  2. Jun 21:08 tomcat-i18n-fr.jar
-rw-r----- 1 tomcat tomcat  190682  2. Jun 21:08 tomcat-i18n-ja.jar
-rw-r----- 1 tomcat tomcat  191243  2. Jun 21:08 tomcat-i18n-ko.jar
-rw-r----- 1 tomcat tomcat   52935  2. Jun 21:08 tomcat-i18n-pt-BR.jar
-rw-r----- 1 tomcat tomcat   50259  2. Jun 21:08 tomcat-i18n-ru.jar
-rw-r----- 1 tomcat tomcat  174396  2. Jun 21:08 tomcat-i18n-zh-CN.jar
-rw-r----- 1 tomcat tomcat  149544  2. Jun 21:08 tomcat-jdbc.jar
-rw-r----- 1 tomcat tomcat   37872  2. Jun 21:08 tomcat-jni.jar
-rw-r----- 1 tomcat tomcat  215612  2. Jun 21:08 tomcat-util.jar
-rw-r----- 1 tomcat tomcat  224215  2. Jun 21:08 tomcat-util-scan.jar
-rw-r----- 1 tomcat tomcat  241496  2. Jun 21:08 tomcat-websocket.jar
-rw-r----- 1 tomcat tomcat   39977  2. Jun 21:08 websocket-api.jar
```


## MIR Webapp deployment and configuration

See <https://www.mycore.de/site/download/dl_mir/mir_release/>.

Download LTS version from <https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=org.mycore.mir&a=mir-webapp&v=2021.06.1-SNAPSHOT&e=war> and save it as `mir.war` (175 MB) into Apache Tomcat `webapps` directory:

```
$ sudo su - tomcat -c "wget 'https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=org.mycore.mir&a=mir-webapp&v=2021.06.1-SNAPSHOT&e=war' -O /opt/apache-tomcat-9.0.64/webapps/mir.war"
```

Start Tomcat:

```
$ sudo -u tomcat /opt/apache-tomcat-9.0.64/bin/catalina.sh start
```

Browse to MIR, starting configuration with the MIR Wizard: <http://localhost:8081/mir>

### Step 1

![MIR Wizard - Step 1](/assets/topics/server/mycore/mir-wizard-01.png)

See <https://www.mycore.de/documentation/apps/mir/mir_install/> section "MIR-Wizard".

To get the Token, open Apache Tomcat's `caralina.out` log file and search for `Login token`:

```
$ sudo -u tomcat less /opt/apache-tomcat-9.0.64/logs/catalina.out
...
================================================================================
 MIR Wizard
================================================================================

 ESC[41mESC[1;37mLogin token: 2d420eb0-1574-49cf-9c22-e70a210e2406ESC[m

================================================================================
```

After cleaning up (remove escape characters), the token is `2d420eb0-1574-49cf-9c22-e70a210e2406`

Put the token value into field `Token` and click `Anmelden`.

### Step 2

In the next step enter Solr and Postgres-Database data:

![MIR Wizard - Step 1](/assets/topics/server/mycore/mir-wizard-02.png)

* SOLR-Server:
  * URL: http://localhost/8983
  * Anwendungs-Kern: mir-main
  * Klassifikationen-Kern: mir-class
* SMTP-Einstellungen: (empty for now)
* Datenbank:
  * Typ: PostgreSQL
  * URL: jdbc:postgresql://127.0.0.1/mir
  * Benutzername: mir
  * Passwort: **** (password given on creation of database)

Click on `Speichern`.

### Step 3

Now MIR Wizard configures the webapp and prints a report of taken actions:

![MIR Wizard - Step 1](/assets/topics/server/mycore/mir-wizard-03.png)

Everything seems to be ok, but some SOLR errors occured:

```
2022-06-19T17:27:02,059 INFO - Load schema definitions for core main using configuration main
2022-06-19T17:27:07,477 ERROR - SOLR schema add error: 400 Bad Request
{
  "responseHeader":{
    "status":400,
    "QTime":9},
  "error":{
    "metadata":[
      "error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject",
      "root-error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject"],
    "details":[{
        "add-field-type":{
          "name":"alto_continuous_text",
          "class":"solr.TextField",
          "indexed":true,
          "stored":true,
          "indexAnalyzer":{
            "type":"index",
            "tokenizer":{"class":"solr.WhitespaceTokenizerFactory"},
            "filters":[{"class":"solr.LowerCaseFilterFactory"},
              {
                "class":"solr.StopFilterFactory",
                "ignoreCase":"true",
                "words":"stopwords.txt"},
              {"class":"solr.ASCIIFoldingFilterFactory"},
              {"class":"solr.ICUNormalizer2FilterFactory"},
              {"class":"solr.ApostropheFilterFactory"},
              {
                "class":"solr.PatternReplaceCharFilterFactory",
                "pattern":"[^\\p{L}\\p{Nd}]",
                "replacement":""},
              {
                "class":"solr.SnowballPorterFilterFactory",
                "language":"German2"}]},
          "queryAnalyzer":{
            "type":"query",
            "tokenizer":{"class":"solr.WhitespaceTokenizerFactory"},
            "filters":[{"class":"solr.LowerCaseFilterFactory"},
              {
                "class":"solr.StopFilterFactory",
                "ignoreCase":"true",
                "words":"stopwords.txt"},
              {"class":"solr.ASCIIFoldingFilterFactory"},
              {"class":"solr.ICUNormalizer2FilterFactory"},
              {
                "class":"solr.SnowballPorterFilterFactory",
                "language":"German2"}]}},
        "errorMessages":["Plugin init failure for [schema.xml] fieldType\nPlugin init failure for [schema.xml] analyzer/filter:  Error loading class 'solr.ICUNormalizer2FilterFactory'\n Error loading class 'solr.ICUNormalizer2FilterFactory'\nsolr.ICUNormalizer2FilterFactory\n"]}],
    "msg":"error processing commands, errors: [{add-field-type={name=alto_continuous_text, class=solr.TextField, indexed=true, stored=true, indexAnalyzer={type=index, tokenizer={class=solr.WhitespaceTokenizerFactory}, filters=[{class=solr.LowerCaseFilterFactory}, {class=solr.StopFilterFactory, ignoreCase=true, words=stopwords.txt}, {class=solr.ASCIIFoldingFilterFactory}, {class=solr.ICUNormalizer2FilterFactory}, {class=solr.ApostropheFilterFactory}, {class=solr.PatternReplaceCharFilterFactory, pattern=[^\\p{L}\\p{Nd}], replacement=}, {class=solr.SnowballPorterFilterFactory, language=German2}]}, queryAnalyzer={type=query, tokenizer={class=solr.WhitespaceTokenizerFactory}, filters=[{class=solr.LowerCaseFilterFactory}, {class=solr.StopFilterFactory, ignoreCase=true, words=stopwords.txt}, {class=solr.ASCIIFoldingFilterFactory}, {class=solr.ICUNormalizer2FilterFactory}, {class=solr.SnowballPorterFilterFactory, language=German2}]}}, errorMessages=[Plugin init failure for [schema.xml] fieldType\nPlugin init failure for [schema.xml] analyzer/filter:  Error loading class 'solr.ICUNormalizer2FilterFactory'\n Error loading class 'solr.ICUNormalizer2FilterFactory'\nsolr.ICUNormalizer2FilterFactory\n]}], ",
    "code":400}}

2022-06-19T17:27:07,484 ERROR - SOLR schema add error: 400 Bad Request
{
  "responseHeader":{
    "status":400,
    "QTime":4},
  "error":{
    "metadata":[
      "error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject",
      "root-error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject"],
    "details":[{
        "add-field-type":{
          "name":"alto_word_coordinates",
          "class":"solr.TextField",
          "indexed":true,
          "multiValued":true,
          "stored":true,
          "indexAnalyzer":{
            "type":"index",
            "tokenizer":{"class":"solr.WhitespaceTokenizerFactory"},
            "filters":[{
                "class":"solr.DelimitedPayloadTokenFilterFactory",
                "delimiter":"|",
                "encoder":"identity"},
              {"class":"solr.LowerCaseFilterFactory"},
              {
                "class":"solr.StopFilterFactory",
                "ignoreCase":"true",
                "words":"stopwords.txt"},
              {"class":"solr.ASCIIFoldingFilterFactory"},
              {"class":"solr.ICUNormalizer2FilterFactory"},
              {"class":"solr.ApostropheFilterFactory"},
              {
                "class":"solr.PatternReplaceCharFilterFactory",
                "pattern":"[^\\p{L}\\p{Nd}]",
                "replacement":""},
              {
                "class":"solr.SnowballPorterFilterFactory",
                "language":"German2"}]},
          "queryAnalyzer":{
            "type":"query",
            "tokenizer":{"class":"solr.WhitespaceTokenizerFactory"},
            "filters":[{"class":"solr.LowerCaseFilterFactory"},
              {
                "class":"solr.StopFilterFactory",
                "ignoreCase":"true",
                "words":"stopwords.txt"},
              {"class":"solr.ASCIIFoldingFilterFactory"},
              {"class":"solr.ICUNormalizer2FilterFactory"},
              {
                "class":"solr.SnowballPorterFilterFactory",
                "language":"German2"}]}},
        "errorMessages":["Plugin init failure for [schema.xml] fieldType\nPlugin init failure for [schema.xml] analyzer/filter:  Error loading class 'solr.ICUNormalizer2FilterFactory'\n Error loading class 'solr.ICUNormalizer2FilterFactory'\nsolr.ICUNormalizer2FilterFactory\n"]}],
    "msg":"error processing commands, errors: [{add-field-type={name=alto_word_coordinates, class=solr.TextField, indexed=true, multiValued=true, stored=true, indexAnalyzer={type=index, tokenizer={class=solr.WhitespaceTokenizerFactory}, filters=[{class=solr.DelimitedPayloadTokenFilterFactory, delimiter=|, encoder=identity}, {class=solr.LowerCaseFilterFactory}, {class=solr.StopFilterFactory, ignoreCase=true, words=stopwords.txt}, {class=solr.ASCIIFoldingFilterFactory}, {class=solr.ICUNormalizer2FilterFactory}, {class=solr.ApostropheFilterFactory}, {class=solr.PatternReplaceCharFilterFactory, pattern=[^\\p{L}\\p{Nd}], replacement=}, {class=solr.SnowballPorterFilterFactory, language=German2}]}, queryAnalyzer={type=query, tokenizer={class=solr.WhitespaceTokenizerFactory}, filters=[{class=solr.LowerCaseFilterFactory}, {class=solr.StopFilterFactory, ignoreCase=true, words=stopwords.txt}, {class=solr.ASCIIFoldingFilterFactory}, {class=solr.ICUNormalizer2FilterFactory}, {class=solr.SnowballPorterFilterFactory, language=German2}]}}, errorMessages=[Plugin init failure for [schema.xml] fieldType\nPlugin init failure for [schema.xml] analyzer/filter:  Error loading class 'solr.ICUNormalizer2FilterFactory'\n Error loading class 'solr.ICUNormalizer2FilterFactory'\nsolr.ICUNormalizer2FilterFactory\n]}], ",
    "code":400}}

2022-06-19T17:27:07,486 ERROR - SOLR schema add error: 400 Bad Request
{
  "responseHeader":{
    "status":400,
    "QTime":0},
  "error":{
    "metadata":[
      "error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject",
      "root-error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject"],
    "details":[{
        "add-field":{
          "name":"alto_content",
          "type":"alto_continuous_text"},
        "errorMessages":["Field 'alto_content': Field type 'alto_continuous_text' not found.\n"]}],
    "msg":"error processing commands, errors: [{add-field={name=alto_content, type=alto_continuous_text}, errorMessages=[Field 'alto_content': Field type 'alto_continuous_text' not found.\n]}], ",
    "code":400}}

2022-06-19T17:27:07,489 ERROR - SOLR schema add error: 400 Bad Request
{
  "responseHeader":{
    "status":400,
    "QTime":0},
  "error":{
    "metadata":[
      "error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject",
      "root-error-class","org.apache.solr.api.ApiBag$ExceptionWithErrObject"],
    "details":[{
        "add-field":{
          "name":"alto_words",
          "type":"alto_word_coordinates",
          "multiValued":true},
        "errorMessages":["Field 'alto_words': Field type 'alto_word_coordinates' not found.\n"]}],
    "msg":"error processing commands, errors: [{add-field={name=alto_words, type=alto_word_coordinates, multiValued=true}, errorMessages=[Field 'alto_words': Field type 'alto_word_coordinates' not found.\n]}], ",
    "code":400}}
```

Again this might be an issue of Solr 9. The class `ICUNormalizer2FilterFactory` can be found in module `analysis-extras` in JAR
`/opt/solr-9.0.0/modules/analysis-extras/lib/lucene-analysis-icu-9.0.0.jar` under class path: `org.apache.lucene.analysis.icu.ICUNormalizer2FilterFactory`.

FIXME: where to fix the new package path?

So we have to add module `analysis-extras` to Solr startup: `-Dsolr.modules=scripting,analysis-extras`.
We mention this above in the Solr section.

Click on `Server herunterfahren` and stop Tomcat server.

### Stop and Start MIR Webapp

Restart Solr with module `analysis-extras` activated:

```
$ sudo -u solr /opt/solr-9.0.0/bin/solr stop -all
$ sudo -u solr /opt/solr-9.0.0/bin/solr start -s /home/solr/cores -Dsolr.log.dir=/home/solr/logs -Dsolr.modules=scripting,analysis-extras
```

Start tomcat again.

```
$ sudo -u tomcat /opt/apache-tomcat-9.0.64/bin/catalina.sh stop
$ sudo -u tomcat /opt/apache-tomcat-9.0.64/bin/catalina.sh start
```

Browse <http://localhost:8081/mir>

![MIR Webapp](/assets/topics/server/mycore/mir-webapp-01.png)

Until now now documents are found (execute empty search) as we did not import objects, yet:

![MIR Webapp - Empty search](/assets/topics/server/mycore/mir-webapp-02.png)

## MIR Command Line Client

* Homepage: <https://www.mycore.de/documentation/cli/cli_overview/>
* Sourcecode: <https://github.com/MyCoRe-Org/mir/tree/2021.06.x/mir-cli>
* Download: <https://www.mycore.de/site/download/dl_mir/mir_release/>

Download LTS version from <https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=org.mycore.mir&a=mir-cli&v=2021.06.1-SNAPSHOT&e=tar.gz> and save it as `mir-cli` under

```
$ cd ~/Downloads
$ wget "https://oss.sonatype.org/service/local/artifact/maven/redirect?r=snapshots&g=org.mycore.mir&a=mir-cli&v=2021.06.1-SNAPSHOT&e=tar.gz" -O mir-cli-2021.06.1-SNAPSHOT.tar.gz
```

Unpack it into a directory you have proper permissions of:

```
$ cd ~/development/programs/
$ tar xvfz ~/Downloads/mir-cli-2021.06.1-SNAPSHOT.tar.gz
$ ls -al ~/development/programs/mir-cli-2021.06.1-SNAPSHOT/
insgesamt 68
drwxr-xr-x  5 ralf ralf  4096 19. Jun 16:55 .
drwxr-xr-x 23 ralf ralf  4096 19. Jun 16:55 ..
drwxr-xr-x  2 ralf ralf  4096 19. Jun 16:56 bin
drwxr-x---  3 ralf ralf  4096 24. Mai 16:37 config
drwxr-xr-x  2 ralf ralf 16384 24. Mai 16:57 lib
-rw-r-----  1 ralf ralf 35147 24. Mai 16:37 LICENSE.txt
```

