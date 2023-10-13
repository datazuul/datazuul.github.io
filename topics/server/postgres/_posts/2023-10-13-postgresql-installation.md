---
layout: post
title: PostgreSql - Installation, Configuration, Operations
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://www.postgresql.org>
* Version: 15
* Documentation: <https://www.postgresql.org/docs/15/>

# Installation

## Ubuntu 23.04

```
$ sudo apt-cache search postgresql
...
postgresql - Objektrelationale SQL-Datenbank (unterstützte Version)
...
$ sudo apt install postgresql
```

Test:

```
$ sudo su - postgres
$ psql --version
psql (PostgreSQL) 15.4 (Ubuntu 15.4-0ubuntu0.23.04.1)
$
```
