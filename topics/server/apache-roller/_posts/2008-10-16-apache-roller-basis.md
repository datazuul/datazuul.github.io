---
layout: post
title: Apache Roller 4.0.0 - Installation, Konfiguration, Betrieb
author: Ralf Eichinger
toc: true
---

# Installation und Konfiguration

Diese Anleitung beschreibt die Installation von Apache Roller unter Ubuntu Linux in Apache Tomcat 5.5.

## Abhängigkeiten installieren

### MySQL-JDBC:

```sh
# apt-get install libmysql-java
# cp /usr/share/java/mysql-connector-java-5.0.4.jar \
    /var/lib/tomcat5.5/shared/lib/
```

### Javamail

Java < 1.6: JavaBeans Activation Framework benötigt
Download von http://java.sun.com/javase/technologies/desktop/javabeans/jaf/index.jsp

```sh
# unzip jaf-1_1_1.zip
# cp jaf-1.1.1/activation.jar /var/lib/tomcat5.5/shared/lib/
```

Download von http://java.sun.com/products/javamail/

```sh
# unzip javamail-1_4_1.zip
# cp javamail-1.4.1/mail.jar /var/lib/tomcat5.5/shared/lib/
```

## Konfiguration Tomcat

### UTF-8 Encoding für Tomcat:

```sh
# nano /var/lib/tomcat5.5/conf/server.xml
<Connector port="8180" maxHttpHeaderSize="8192"
           maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
           enableLookups="false" redirectPort="8443" acceptCount="100"
           connectionTimeout="20000" disableUploadTimeout="true"
           URIEncoding="UTF-8" />
... bei allen Connectoren...
```

Tomcat restarten:

```sh
$ sudo /etc/init.d/tomcat5.5 restart
```

## Installation Apache Roller

Download the Apache Roller release file (Roller-WAR)  from http://roller.apache.org

```sh
# wget http://apache.autinity.de/roller/roller-4/v4.0.0\
         /bin/apache-roller-4.0.zip
# unzip apache-roller-4.0.zip
# cd apache-roller-4.0/webapp/roller
```

Da jar-Befehl fehlt:

```sh
# apt-get install sun-java5-jdk

# jar cvf ../roller.war *
# cd ..
# ls -al
insgesamt 23504
drwxr-xr-x 3 root root     4096 2008-10-16 10:46 .
drwxr-xr-x 4 root root     4096 2007-11-20 15:34 ..
drwxr-xr-x 8 root root     4096 2007-11-20 15:33 roller
-rw-r--r-- 1 root root 24025726 2008-10-16 10:46 roller.war
#
```

## Installation MySQL-Database

```sh
# mysql -u root -p

mysql> create database rollerdb;
Query OK, 1 row affected (0.00 sec)

mysql> grant all on rollerdb.* to roller@'%' identified by 'blader';
Query OK, 0 rows affected (0.03 sec)

mysql> grant all on rollerdb.* to roller@localhost identified by 'blader';
Query OK, 0 rows affected (0.00 sec)
```

## Konfiguration Apache Roller

Roller-Config:

```sh
# cd ../..
# nano roller-custom.properties
installation.type=auto
database.configurationType=jdbc
database.jdbc.driverClass=com.mysql.jdbc.Driver
database.jdbc.connectionURL=jdbc:mysql://localhost:3306/rollerdb
database.jdbc.username=roller
database.jdbc.password=blader
mail.configurationType=properties
mail.hostname=mail.pixotec.de
mail.username=ralf.eichinger@pixotec.de
mail.password=clearpass
```

```sh
# mv roller-custom.properties /var/lib/tomcat5.5/shared/classes/
# chown -R tomcat55:adm /var/lib/tomcat5.5/shared/
```

Roller-Deployment:

```sh
# mv apache-roller-4.0/webapp/roller.war /var/lib/tomcat5.5/webapps/
# /etc/init.d/tomcat5.5 restart
```

## Der erste Start

Aufruf: <http://www.pixoblog.de:8180/roller/>

```
--> No database tables found
Yes - create tables now

Tables created successfully

Click here to complete the installation process and start using Roller. 
```

Exception:

```sh
# less /var/log/tomcat5.5/roller.log
java.io.IOException: Cannot create directory: \
  /usr/share/tomcat5.5/roller_data/search-index

# mkdir -p /usr/share/tomcat5.5/roller_data/search-index
# chown tomcat55:adm /usr/share/tomcat5.5/roller_data/search-index

# /etc/init.d/tomcat55 restart
```

Aufruf <http://www.pixoblog.de:8180/roller/>


If Roller starts up fine but doesn't find a front-page weblog then it will display the Completing Your Installation below that explains how to register your first user, create your first weblog and setup your site's front page.

- Create a user (the first will be admin)
roller/blader

- Create a weblog
If you are using Roller to run a community of multiple weblogs, then you'll probably want to display an
aggregated front-page combining all weblogs on the site. In that case, create a weblog to serve as the front-page,
set it as the front-page weblog and make sure you set the aggregated front-page setting on the Server Admin
page.

handle: URL:  http://www.pixoblog.de:8180/roller/blogs

Server administration:
Handle of weblog to serve as frontpage blog: blogs
Enable aggregated site-wide frontpage: yes
Sichern

Frontpage, Ausloggen


Now that you're done with the installation you should turn off Roller's auto-installation system. Edit your
roller-custom.properties file and set installation.type=manual. Then restart your server or
Roller so that it accepts the new setting.

```sh
# nano ../../shared/classes/roller-custom.properties
```
