---
layout: post
title: PostgreSql - Installation, Configuration, Usage
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://www.postgresql.org>
* Version: 15
* Documentation: <https://www.postgresql.org/docs/15/>

# Installation

## Ubuntu 23.04

```shell
$ sudo apt-cache search postgresql | sort | grep postgresql
...
postgresql - Objektrelationale SQL-Datenbank (unterstützte Version)
...
postgresql-15 - Die weltweit fortschrittlichste relationale Open-Source-Datenbank
...
$ sudo apt install postgresql
```

Test:

```shell
$ sudo su - postgres
postgres$ psql --version
psql (PostgreSQL) 15.4 (Ubuntu 15.4-0ubuntu0.23.04.1)
```

# Configuration

no configuration changes needed, yet.

# Usage

## Create an user for a database

```sh
$ sudo su - postgres
postgres$ psql -c "CREATE USER cudami PASSWORD 'somepassword';"
CREATE ROLE
```

## Create a database

Let's create a database called "alexandria", owned by user "cudami" using german locale and collation (see <https://www.postgresql.org/docs/15/collation.html>).

    The collation feature allows specifying the sort order and character classification behavior of data per-column, or even per-operation.
    This alleviates the restriction that the LC_COLLATE and LC_CTYPE settings of a database cannot be changed after its creation.

Locales of your system:

```shell
$ locale -a
C
C.utf8
...
de_DE.utf8
...
en_US.utf8
en_ZA.utf8
en_ZM
en_ZM.utf8
en_ZW.utf8
POSIX
```

Create database (see <https://www.postgresql.org/docs/15/app-createdb.html>):

```shell
$ sudo su - postgres
(postgres$ dropdb 'alexandria')
postgres$ createdb alexandria -O cudami --lc-collate=de_DE.UTF-8 --lc-ctype=de_DE.UTF-8
```

## List databases

```sh
postgres$ psql -l
                                                  List of databases
    Name    |  Owner   | Encoding |   Collate   |    Ctype    | ICU Locale | Locale Provider |   Access privileges   
------------+----------+----------+-------------+-------------+------------+-----------------+-----------------------
 alexandria | cudami   | UTF8     | de_DE.UTF-8 | de_DE.UTF-8 |            | libc            | 
 postgres   | postgres | UTF8     | de_DE.UTF-8 | de_DE.UTF-8 |            | libc            | 
 template0  | postgres | UTF8     | de_DE.UTF-8 | de_DE.UTF-8 |            | libc            | =c/postgres          +
            |          |          |             |             |            |                 | postgres=CTc/postgres
 template1  | postgres | UTF8     | de_DE.UTF-8 | de_DE.UTF-8 |            | libc            | =c/postgres          +
            |          |          |             |             |            |                 | postgres=CTc/postgres
(4 rows)
```

## Connect to database and list tables

```shell
postgres$ psql -d alexandria
psql (15.4 (Ubuntu 15.4-0ubuntu0.23.04.1))
Type "help" for help.

alexandria=# \d
Did not find any relations.
alexandria=# \q
```