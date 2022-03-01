---
layout: post
title: Apache 2 Webserver - Installation, Konfiguration, Programmierung, Betrieb
author: Ralf Eichinger
toc: true
---

# Installation

## Ubuntu

Mit „gksudo synaptic“ Paket „apache2“ installieren.

# Konfiguration

## Konfigurationsdateien

Speicherort: /etc/apache2 (Ubuntu)  
Dateien: apache2.conf, httpd.conf, ports.conf  
Verzeichnisse: mods-enabled, sites-enabled

### Module

Module erweitern die Funktionalität des Apache. Es existieren eine Vielzahl von Modulen für die unterschiedlichsten Funktionalitäten.

Der Apache aktiviert automatisch alle Module, deren Konfigurationsdateien (*.load und *.conf) sich im Verzeichnis „/etc/apache2/mods-enabled“ befinden. Dies geschieht durch die Anweisungen

```
# Include module configuration:
Include /etc/apache2/mods-enabled/*.load
Include /etc/apache2/mods-enabled/*.conf
```

in der Konfigurationsdatei „/etc/apache2/apache2.conf“.

Konfigurationsdateien aller verfügbaren Module befinden sich im Verzeichnis „/etc/apache2/mods-available“.  
Bei einer Standardinstallation des Apache sind folgende Modul-Konfigurationsdateien verfügbar:

```sh
-rw-r--r-- 1 root root   66 2006-09-27 18:54 actions.load
-rw-r--r-- 1 root root   60 2006-09-27 18:54 asis.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 auth_anon.load
-rw-r--r-- 1 root root   68 2006-09-27 18:54 auth_dbm.load
-rw-r--r-- 1 root root   74 2006-09-27 18:54 auth_digest.load
-rw-r--r-- 1 root root  130 2006-09-27 18:54 auth_ldap.load
-rw-r--r-- 1 root root   62 2006-09-27 18:54 cache.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 cern_meta.load
-rw-r--r-- 1 root root   61 2006-09-27 18:54 cgid.conf
-rw-r--r-- 1 root root   60 2006-09-27 18:54 cgid.load
-rw-r--r-- 1 root root   58 2006-09-27 18:54 cgi.load
-rw-r--r-- 1 root root   37 2006-09-27 18:54 dav_fs.conf
-rw-r--r-- 1 root root   64 2006-09-27 18:54 dav_fs.load
-rw-r--r-- 1 root root   58 2006-09-27 18:54 dav.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 deflate.load
-rw-r--r-- 1 root root   72 2006-09-27 18:54 disk_cache.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 expires.load
-rw-r--r-- 1 root root   72 2006-09-27 18:54 ext_filter.load
-rw-r--r-- 1 root root   72 2006-09-27 18:54 file_cache.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 headers.load
-rw-r--r-- 1 root root   60 2006-09-27 18:54 imap.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 include.load
-rw-r--r-- 1 root root   60 2006-09-27 18:54 info.load
-rw-r--r-- 1 root root   60 2006-09-27 18:54 ldap.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 mem_cache.load
-rw-r--r-- 1 root root   85 2006-09-27 18:54 mime_magic.conf
-rw-r--r-- 1 root root   72 2006-09-27 18:54 mime_magic.load
-rw-r--r-- 1 root root  840 2006-09-27 18:54 proxy.conf
-rw-r--r-- 1 root root   78 2006-09-27 18:54 proxy_connect.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 proxy_ftp.load
-rw-r--r-- 1 root root   72 2006-09-27 18:54 proxy_http.load
-rw-r--r-- 1 root root  316 2006-09-27 18:54 proxy.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 rewrite.load
-rw-r--r-- 1 root root   66 2006-09-27 18:54 speling.load
-rw-r--r-- 1 root root 3545 2006-09-27 18:54 ssl.conf
-rw-r--r-- 1 root root   58 2006-09-27 18:54 ssl.load
-rw-r--r-- 1 root root   64 2006-09-27 18:54 suexec.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 unique_id.load
-rw-r--r-- 1 root root  244 2006-09-27 18:54 userdir.conf
-rw-r--r-- 1 root root   66 2006-09-27 18:54 userdir.load
-rw-r--r-- 1 root root   70 2006-09-27 18:54 usertrack.load
-rw-r--r-- 1 root root   74 2006-09-27 18:54 vhost_alias.load
```

Durch die Erzeugung eines symbolischen Links auf die „load“- (und „conf“-) Datei eines Moduls im Verzeichnis „/etc/apache2/mods-enabled“ kann ein Modul aktiviert werden.

## Virtuelle Hosts (Domain-Hosting)

Durch die Konfigurationsanweisung

```sh
# Include the virtual host configurations:
Include /etc/apache2/sites-enabled/[^.#]*
```

in der Konfigurationsdatei „/etc/apache2/apache2.conf“ werden alle Dateien im Verzeichnis „/etc/apache2/sites-enabled“ für die Konfiguration von virtuellen Hosts verwendet.

Im Verzeichnis „sites-available“ befinden sich die Konfigurationsdateien der vorkonfigurierten, aber nicht aktiven virtuellen Hosts.

### Der virtuelle Host „localhost“

Nach einer Standardinstallation befindet sich hier die Konfigurationsdatei „default“:

```sh
NameVirtualHost *
<VirtualHost *>
  ServerAdmin webmaster@localhost

  DocumentRoot /var/www
  <Directory />
    Options FollowSymLinks
    AllowOverride None
  </Directory>
  <Directory /var/www/>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
    # Uncomment this directive is you want to see apache2's
    # default start page (in /apache2-default) when you go to /
    #RedirectMatch ^/$ /apache2-default/
  </Directory>

  ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
  <Directory "/usr/lib/cgi-bin">
    AllowOverride None
    Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
    Order allow,deny
    Allow from all
  </Directory>

  ErrorLog /var/log/apache2/error.log

  # Possible values include: debug, info, notice, warn, error, crit,
  # alert, emerg.
  LogLevel warn

  CustomLog /var/log/apache2/access.log combined
  ServerSignature On

  Alias /doc/ "/usr/share/doc/"
  <Directory "/usr/share/doc/">
    Options Indexes MultiViews FollowSymLinks
    AllowOverride None
    Order deny,allow
    Deny from all
    Allow from 127.0.0.0/255.0.0.0 ::1/128
  </Directory>
</VirtualHost>
```

Aktiviert ist diese Konfigurationsdatei, da im Verzeichnis „/etc/apache2/sites-enabled“ ein symbolischer Link („000-default“) auf diese Datei eingerichtet ist:

```sh
root@lenovo:/etc/apache2/sites-enabled# ls -al
drwxr-xr-x 2 root root 4096 2006-10-30 15:33 .
drwxr-xr-x 8 root root 4096 2007-01-26 21:00 ..
lrwxrwxrwx 1 root root   36 2006-10-30 15:33 000-default -> \
  /etc/apache2/sites-available/default
```

Dieser „Default“-Host ist unter „[http://localhost/](http://localhost/)“ erreichbar und zeigt den Inhalt des Verzeichnisses „/var/www“ (der DocumentRoot) an.

<figure>
<img src="{{site.baseurl}}/assets/topics/server/apache-httpd/localhost-001.jpg" alt="Apache localhost directory view" />
</figure>

Unter „[http://localhost/doc/](http://localhost/doc/)“ ist ein Alias auf „/usr/share/doc“ eingerichtet. Es werden alle verfügbaren Dokumentationen des Systems angezeigt, die sich hier befinden:

<figure>
<img src="{{site.baseurl}}/assets/topics/server/apache-httpd/localhost-002.jpg" alt="Apache localhost docs directory view" />
</figure>

Unter „[http://localhost/doc/apache2-doc/manual](http://localhost/doc/apache2-doc/manual)“ ist das Apache-Handbuch abrufbar.

### Einrichtung eines virtuellen Hosts

#### Entwicklung (lokal)

Beispiel: Domain „envi-con-2006.localhost“

Datei „/etc/apache2/sites-available/envi-con-2006.localhost.conf“:

```sh
<VirtualHost *>
  ServerName envicon-2006
  ServerAdmin webmaster@example.org

  DocumentRoot /var/www/envicon-2006
  <Directory />
    Options FollowSymLinks +Includes
    AllowOverride None
  </Directory>
  <Directory /var/www/>
    Options Indexes FollowSymLinks MultiViews +Includes
    AllowOverride None
    Order allow,deny
    allow from all
  </Directory>

  ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
  <Directory "/usr/lib/cgi-bin">
    AllowOverride None
    Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
    Order allow,deny
    Allow from all
  </Directory>

  ErrorLog /var/log/apache2/envicon-2006_error.log

  # Possible values include: debug, info, notice, warn, error, crit,
  # alert, emerg.
  LogLevel warn

  CustomLog /var/log/apache2/envicon-2006_access.log combined
  ServerSignature On
</VirtualHost>
```

Um den virtuellen Host zu aktivieren, genügt es im Verzeichnis „/etc/apache2/sites-enabled“ einen symbolischen Link auf die Datei einzurichten:

```sh
root@lenovo:/etc/apache2/sites-enabled# \
  ln -s /etc/apache2/sites-available/envi-con-2006.localhost.conf \
    001-envi-con-2006.localhost
```

Damit dieser virtualle Host tatsächlich unter „[http://envicon-2006/](http://envi-con-2006.localhost/)“ erreichbar ist, muß der Hostname „envicon-2006“ auflösbar sein. Damit dies lokal geschieht genügt eine Änderung in der Datei „/etc/hosts“:

```sh
...  
127.0.0.1 localhost
127.0.0.1 envicon-2006
...
```

Nach einem Neustart des Apache mit

```sh
# /etc/init.d/apache2 restart
```

ist der neue virtuelle Host unter <http://envicon-2006/> erreichbar.

## SSI (Server Side Includes)

Modul: mod_include  
Speicherort: /usr/lib/apache2/modules/mod_include.so

### Aktivierung

```sh
# ln -s /etc/apache2/mods-available/include.load \
  /etc/apache2/mods-enabled/include.load
```

### Konfiguration

Apache erkennt SSI-Dateien, wenn sie die Endung *.shtml besitzen. Damit „index.shtml“ gleichberechtigt zu „index.html“ als Defaultdatei eines Verzeichnisses verwendet wird, muß folgende Konfiguration vorgenommen werden:

```sh
# vi /etc/apache2/apache2.conf
...
DirectoryIndex index.html index.cgi index.pl \
  index.php index.xhtml index.shtml
...
```

Bereits vorkonfiguriert ist die Verwendung des Outputfilters „INCLUDES“ für *.shtml-Dateien:

```sh
...
# To use server-parsed HTML files
#
<FilesMatch "\.shtml(\..+)?$">
  SetOutputFilter INCLUDES
</FilesMatch>
...
```

Desweiteren muß die Direktive „Options +Includes“ in der Hostkonfiguration hinzugefügt werden:

```sh
<Directory />
  Options FollowSymLinks +Includes
  AllowOverride None
</Directory>
<Directory /var/www/>
  Options Indexes FollowSymLinks MultiViews +Includes
  AllowOverride None
  Order allow,deny
  allow from all
</Directory>
```