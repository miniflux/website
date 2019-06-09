title: Command Line Usage
description: List of available command line arguments for Miniflux
template: doc
uri: docs/cli.html
---
<h2 id="version">Show Application Version <a class="anchor" href="#version" title="Permalink">¶</a></h2>

```bash
miniflux -version # or -v
2.0.15
```

<h2 id="info">Show Build Information <a class="anchor" href="#info" title="Permalink">¶</a></h2>

```bash
miniflux -info # or -i
Version: 2.0.15
Build Date: 2019-03-16T18:26:30-0700
Go Version: go1.12
Compiler: gc
Arch: amd64
OS: darwin
```

<h2 id="debug">Enable Debug Mode <a class="anchor" href="#debug" title="Permalink">¶</a></h2>

```bash
miniflux -debug
[INFO] Debug mode enabled
[INFO] Starting Miniflux...
```

<h2 id="migrate">Run Database Migrations <a class="anchor" href="#migrate" title="Permalink">¶</a></h2>

```bash
export DATABASE_URL=replace_me

miniflux -migrate
Current schema version: 0
Latest schema version: 12
Migrating to version: 1
Migrating to version: 2
Migrating to version: 3
Migrating to version: 4
Migrating to version: 5
Migrating to version: 6
Migrating to version: 7
Migrating to version: 8
Migrating to version: 9
Migrating to version: 10
Migrating to version: 11
Migrating to version: 12
```

<h2 id="create-admin">Create Admin User <a class="anchor" href="#create-admin" title="Permalink">¶</a></h2>

```bash
miniflux -create-admin
Enter Username: root
Enter Password: ****
```

<h2 id="reset-password">Reset User Password <a class="anchor" href="#reset-password" title="Permalink">¶</a></h2>

```bash
miniflux -reset-password
Enter Username: myusername
Enter Password: ****
```

<h2 id="flush-sessions">Flush All Sessions <a class="anchor" href="#flush-sessions" title="Permalink">¶</a></h2>

```bash
miniflux -flush-sessions
Flushing all sessions (disconnect users)
```

<h2 id="reset-feed-errors">Reset All Feed Errors <a class="anchor" href="#reset-feed-errors" title="Permalink">¶</a></h2>

```bash
miniflux -reset-feed-errors
```

<h2 id="config-file">Use a Configuration File <a class="anchor" href="#config-file" title="Permalink">¶</a></h2>

```bash
miniflux -config-file /etc/miniflux.conf
```

or 

```bash
miniflux -c /etc/miniflux.conf
```

<h2 id="config-dump">Show Interpreted Configuration Values <a class="anchor" href="#config-dump" title="Permalink">¶</a></h2>

```bash
miniflux -config-dump
```
