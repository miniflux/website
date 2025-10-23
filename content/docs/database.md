---
title: Database Configuration
description: Instructions to configure PostgreSQL for Miniflux
url: docs/database.html
---

- [PostgreSQL Installation](#installation)
- [PostgreSQL Configuration](#configuration)
- [Database Connection Parameters](#dsn)

This document describes how to configure PostgreSQL for Miniflux.

<h2 id="installation">Database Installation <a class="anchor" href="#installation" title="Permalink">¶</a></h2>

The first step is to install PostgreSQL with your package manager.

For example, on Debian it's as simple as typing this command:

```bash
sudo apt install postgresql postgresql-contrib
```

<h2 id="configuration">Database Configuration <a class="anchor" href="#configuration" title="Permalink">¶</a></h2>

Older versions of Miniflux required the `HSTORE` extension for PostgreSQL.
This is no longer the case since version 2.0.27.

There is a SQL migration in Miniflux 2.2.14 to remove `HSTORE` extension from the database.

If you are seeing this Postgres error: `Error: pq: must be owner of extension hstore`, you can fix it by running the following SQL command as a superuser for the Miniflux database:

```sql
DROP EXTENSION hstore;
```

This error means you initially created the `hstore` extension as a different database user than the one you are currently using for Miniflux.

<h2 id="dsn">Database Connection Parameters <a class="anchor" href="#dsn" title="Permalink">¶</a></h2>

Miniflux uses the Golang library [pq](https://github.com/lib/pq) to communicate with PostgreSQL.
The list of connection parameters are available on [this page](https://pkg.go.dev/github.com/lib/pq?utm_source=godoc#hdr-Connection_String_Parameters).

The default value for `DATABASE_URL` is `user=postgres password=postgres dbname=miniflux2 sslmode=disable`.

You could also use the URL format `postgres://postgres:postgres@localhost/miniflux2?sslmode=disable`.

<div class="warning">
Password that contains special characters like ^ might be rejected since Miniflux 2.0.3. <a href="https://go-review.googlesource.com/c/go/+/87038">Golang v1.10 is now validating the password</a> and will return this error: <code>net/url: invalid userinfo</code>.
To avoid this issue, do not use the URL format for <code>DATABASE_URL</code>, or make sure the password is URL encoded.
</div>
