---
title: Manual Binary Installation
description: Instructions to install Miniflux binary manually
url: docs/binary_installation.html
---

This document describes how to install the Miniflux binary on your Linux server.

Make sure to [install and configure Postgresql]({{< ref "database.md" >}}) before installing Miniflux.

You can download precompiled binaries from the [GitHub Releases page](https://github.com/miniflux/v2/releases). You could also [build the application from the source code]({{< ref "development.md" >}}).

1. Copy the precompiled binary somewhere on your server, for example in `/usr/local/bin`
2. Make the file executable: `chmod +x miniflux`
3. Define the environment variable `DATABASE_URL` if necessary
4. Run the SQL migrations: `miniflux -migrate`
5. Create an admin user: `miniflux -create-admin`
6. Start the application: `miniflux`

<p class="info">
It's recommended to configure a process manager like systemd or supervisord to supervise the Miniflux daemon.
</p>

Make sure to take a look a the list of [configuration parameters]({{< relref configuration >}}) to customize your installation.
