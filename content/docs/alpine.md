---
title: Alpine Linux Installation
description: Instructions to install Miniflux on Alpine Linux
url: docs/alpine.html
---

[Alpine Linux](https://alpinelinux.org/) is a lightweight Linux distribution that is perfectly suited for running Miniflux.

An APK package is available from the community repository (it was in testing before).

Edit the file `/etc/apk/repositories` to enable the Edge repository: `http://dl-cdn.alpinelinux.org/alpine/edge/community`. And then run `apk update`.

The Miniflux installation is simple as running:

```bash
apk add miniflux miniflux-openrc miniflux-doc
```

Do not forget to install Postgresql:

```
apk add postgresql postgresql-contrib
```

Configure the database and enable the `HSTORE` extension as [mentioned previously]({{< ref "database.md" >}}).

On Alpine Linux, the Miniflux process is supervised by `supervise-daemon` from [OpenRC](https://github.com/OpenRC/openrc) (there is no Systemd).
The log file `/var/log/miniflux.log` is rotated with `logrotate`.

In this context, the configuration file `/etc/miniflux.conf` is used instead of environment variables:

```
# /etc/miniflux.conf

LOG_DATE_TIME=yes
LISTEN_ADDR=127.0.0.1:8080
DATABASE_URL=user=postgres password=secret dbname=miniflux sslmode=disable

# Run SQL migrations automatically:
# RUN_MIGRATIONS=1
```

To finalize the installation, create the database schema and a first user:

```bash
miniflux -c /etc/miniflux.conf -migrate
miniflux -c /etc/miniflux.conf -create-admin
```

And finally, start the application:

```bash
service miniflux start
```

Make sure to take a look a the list of [configuration parameters]({{< relref configuration >}}) to customize your installation.
