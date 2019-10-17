---
title: "Basic Guide to PostgreSQL on Arch Linux"
date: 2019-10-17T12:13:38-03:00
draft: false
slug: basic-postgresql-archlinux
tags:
  - Database
  - Reference
  - Arch Linux
  - Guide
---

I'm writing this post as a reference for future usage. I'm installing PostgreSQL to Arch Linux for local development, but eventually I'll write a new guide for CentOS 7 for remote database access.

### Downloading and Installing

Download the PostgreSQL package from a package manager; I'm using yay for this guide:

`yay -S postgres`

### Setup

```
sudo -iu postgres
postgres$ initdb -D /var/lib/postgres/data
postgres$ exit
systemctl start postgres.service
systemctl enable postgres.service
```

Before configuring, be sure to set your password first because we will be using password authentication.

```
postgres$ psql --command '\password postgres'
```

\* if you run into permissions issues with initdb command, try doing a chown command:

`chown -R postgres:postgres /var/lib/postgres/`

### Configuration

edit `/var/lib/postgres/data/pg_hba.conf` to change access method from `trust` or `ident` to `md5` to enable password authentication for local access. Since I'm only using this for local development, I commented out IPv4 and IPv6 access.

### Creating a new user

This createuser command will ask for your password before creating a new user with supplied username:

`createuser --interactive -U postgres -P <username>`

### Creating a new database

This createdb command will ask for your password before creating a new database and make the supplied username the owner of this database:

`createdb -U postgres -O <username> <database>`
