---
layout: post
title: PostgreSql - Backup and Restore of complete database cluster
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://www.postgresql.org>
* Version: 12
* Documentation: <https://www.postgresql.org/docs/12/backup.html>

# Create a Backup

## Single database

Log in to PostgreSql server and dump database `cudami` to a writable directory (e.g. `/tmp`):

```
$ pg_dump cudami > /tmp/postgres-dump-cudami-db-20230803
```

## Complete PostgreSql cluster

Log in to PostgreSql server and dump complete PostgreSql data (databases, roles, tablespaces, ...):

```
$ sudo su - postgres
$ psql --version
psql (12.15 (Debian 12.15-1.pgdg100+1))

$ pg_dumpall > /tmp/postgres-dumpall-20230803
$ ls -al /tmp/postgres-dumpall-20230803 
-rw-r--r-- 1 postgres postgres 2147052 Aug  3 09:07 /tmp/postgres-dumpall-20230803
$
```

# Restore a Backup

## Single database

Copy dump file to the target system for restoring.
Put it in a directory readable by postgres user, e.g. `/tmp`.

Restore database:

```
$ sudo su - postgres
$ dropdb cudami
$ createdb -T template0 cudami
$ psql cudami < /tmp/postgres-dump-cudami-db-20230803
$ psql -c "CREATE USER cudami PASSWORD 'somepassword';"
$ psql -c "GRANT ALL PRIVILEGES ON DATABASE cudami TO cudami;"
GRANT
```

If user/role already exists and you forgot the password:

```
$ psql -c "ALTER ROLE cudami WITH PASSWORD 'somepassword';"
ALTER ROLE
```

## Complete PostgreSql cluster

Copy dump file to the target system for restoring.
Put it in a directory readable by postgres user, e.g. `/tmp`.

Restore complete cluster:

```
$ sudo su - postgres
$ psql -f /tmp/postgres-dumpall-20230803 postgres
```