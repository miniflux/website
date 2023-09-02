---
title: Frequently Asked Questions
url: /faq.html
---

Table of Contents:

- [Why are you not merging my pull-request?](#pull-request)
- [Why are you not developing my feature request?](#feature-request)
- [Feature X was available in Miniflux v1?](#missing-feature-v2)
- [Why Miniflux stores favicons into the database?](#favicons-storage)
- [How to create themes for Miniflux 2?](#themes)
- [Why there is no plugin system?](#plugins)
- [What is "Save this article"?](#save-article)
- [How are items removed from the database?](#entries-suppression)
- [What "Flush History" does?](#flush-history)
- [Which binary do I need to use on my Raspberry Pi?](#arm-pi)
- [Which Debian package architecture should I use for my Raspberry Pi?](#debian-pi-arch)
- [Which binary do I need to use on Scaleway ARM machines?](#arm-scaleway)
- [Which Docker architecture should I use?](#docker-arch)
- [Why SQL migrations are not executed automatically?](#sql-migrations)
- [How to backup my data?](#backup)

<h2 id="pull-request">Why are you not merging my pull-request? <a class="anchor" href="#pull-request" title="Permalink">¶</a></h2>

List of things to avoid:

- Too many changes that would make the pull-request very hard to review
- Breaking changes
- Introduce new bugs, regressions, or security issues
- Add unnecessary dependencies
- Slowing down the software
- Changes are against the philosophy of the software
- Poor code quality
- Chaining pull-requests that depends from each other
- Radical user interface changes

<h2 id="feature-request">Why are you not developing my feature request? <a class="anchor" href="#feature-request" title="Permalink">¶</a></h2>

- Developing a software takes a lot of time.
- This is a free and open source project, no one owes you anything.
- If you miss something, contribute to the project.
- Don't expect anyone to work for free.
- As mentioned previously, the number of features is voluntarily limited. Nobody likes bloatware.
- Improving existing features is more important than adding new ones.

<h2 id="missing-feature-v2">Feature X was available in Miniflux v1? <a class="anchor" href="#missing-feature-v2" title="Permalink">¶</a></h2>

Miniflux 2 doesn't try to reimplement all features of Miniflux 1.
The minimalist approach is pushed a little bit further.

If you really miss something, **you must contribute to the project**, but remember, **you have to keep the minimalist philosophy of Miniflux**.

<h2 id="favicons-storage">Why Miniflux stores favicons into the database? <a class="anchor" href="#favicons-storage" title="Permalink">¶</a></h2>

Miniflux follows the [the Twelve Factors principle](https://12factor.net/).
Nothing is stored on the local file system.
The application is designed to run on ephemeral containers without persistent storage.

<h2 id="themes">How to create themes for Miniflux 2? <a class="anchor" href="#themes" title="Permalink">¶</a></h2>

As of now, Miniflux 2 doesn't have any mechanism to load external stylesheets to avoid dependencies.
Themes are embedded into the binary.

If you would like to submit a new official theme, you must send a pull-request.
But do not forget that **you will have to maintain your theme over the time**, otherwise, your theme will be removed from the code base.

<h2 id="plugins">Why there is no plugin system? <a class="anchor" href="#plugins" title="Permalink">¶</a></h2>

- Because this software has a minimalist approach.
- Because implementing a plugin system increase the complexity of the software.
- Because people do not maintain their plugins after a while.

<h2 id="save-article">What is "Save this article"? <a class="anchor" href="#save-article" title="Permalink">¶</a></h2>

"Save" sends the feed entry to third-party services like Pinboard or Instapaper if configured.

<h2 id="entries-suppression">How are items removed from the database? <a class="anchor" href="#entries-suppression" title="Permalink">¶</a></h2>

Entry status in the database follows this flow: <code>read</code> -> <code>unread</code> -> <code>removed</code>.

Entries marked as removed are not visible in the web ui.
They are deleted from the database only when they are not visible anymore in the original feed.

Entries marked as favorite are never deleted (column <code>starred</code> in the <code>entries</code> table).

Data retention is also controlled with the variables <code>CLEANUP_ARCHIVE_UNREAD_DAYS</code> and <code>CLEANUP_ARCHIVE_READ_DAYS</code>.

Keep in mind that Postgres needs to run the <a href="https://www.postgresql.org/docs/current/sql-vacuum.html">VACUUM</a> command to reclaim disk space.

<h2 id="flush-history">What "Flush History" does? <a class="anchor" href="#flush-history" title="Permalink">¶</a></h2>

"Flush History" changes the status of entries from "read" to "removed" (except for bookmarks).
Entries with the status "removed" are not visible in the user interface.

<h2 id="arm-pi">Which binary do I need to use on my Raspberry Pi? <a class="anchor" href="#arm-pi" title="Permalink">¶</a></h2>

Raspberry Pi Model  | Miniflux Binary
--------------------|---------------------
A, A+, B, B+, Zero  | miniflux-linux-armv6 (32 bits)
2 and 3             | miniflux-linux-armv7 (32 bits)
3 and 4             | miniflux-linux-arm64 (64 bits)

<h2 id="debian-pi-arch">Which Debian package architecture should I use for my Raspberry Pi? <a class="anchor" href="#debian-pi-arch" title="Permalink">¶</a></h2>

Raspberry Pi Model  | Debian Package Architecture
--------------------|---------------------
A, A+, B, B+, Zero  | Not supported yet
2 and 3             | armhf (32 bits)
3 and 4             | arm64 (64 bits)

<h2 id="arm-scaleway">Which binary do I need to use on Scaleway ARM machines? <a class="anchor" href="#arm-scaleway" title="Permalink">¶</a></h2>

Server Type    | Miniflux Binary       | uname -m
---------------|-----------------------|---------
Scaleway C1    | miniflux-linux-armv6  |  armv7l
Scaleway ARM64 | miniflux-linux-armv8  |  aarch64

<h2 id="docker-arch">Which Docker architecture should I use? <a class="anchor" href="#docker-arch" title="Permalink">¶</a></h2>

Pulling the `latest` tag or a specific version should download the correct image according to your machine.

Docker Architecture | uname -m | Example
--------------------|----------|---------------
amd64               |  x86_64  |
arm32v6             |  armhf   | Raspberry Pi
arm32v6             |  armv7l  | Scaleway C1
arm64v8             |  aarch64 | Scaleway ARM64

If you use the wrong architecture, Docker will returns an error like this one:

```
standard_init_linux.go:178: exec user process caused "exec format error"
```

<p class="info">Multi-arch Docker images are available only since Miniflux 2.0.12.</p>

<h2 id="sql-migrations">Why SQL migrations are not executed automatically? <a class="anchor" href="#sql-migrations" title="Permalink">¶</a></h2>

- Because it's a source of problems.
- Only one process should manipulate the database schema at once.
- If you run multiple containers with an orchestrator that may cause issues.
- You can still run the migrations by defining the variable `RUN_MIGRATIONS=1`.

<h2 id="backup">How to backup my data? <a class="anchor" href="#backup" title="Permalink">¶</a></h2>

Refer to the [official Postgres documentation](https://www.postgresql.org/docs/current/backup.html) for details about backing up and restoring a Postgres database.

Basic SQL dump example:

```bash
# Backup
pg_dump miniflux2 -f miniflux.dump

# Restore
psql miniflux2 < miniflux.dump
```

There many other options available, refer to the official documentation of `pg_dump` and `pg_restore`.
