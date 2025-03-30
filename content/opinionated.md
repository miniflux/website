---
title: Opinionated?
description: What makes this software different?
url: opinionated.html
---

Miniflux is a minimalist software. **The purpose of this application is to read feeds**. _Nothing else_.

<h2 id="simplicity">Focus on Simplicity <a class="anchor" href="#simplicity" title="Permalink">¶</a></h2>

- Having a gazillion features makes the software hard to maintain, troubleshoot, and increases the number of bugs.
- This software doesn't try to satisfy everyone's needs.

<h2 id="ui">Why is the user interface ugly? <a class="anchor" href="#ui" title="Permalink">¶</a></h2>

- The Miniflux layout is optimized for quickly scanning entries.
- The design of Miniflux is inspired by [Hacker News](https://news.ycombinator.com/), [Lobsters](https://lobste.rs/), and [Pinboard](https://pinboard.in/).

<h2 id="feature-request">Why are you not developing my feature request? <a class="anchor" href="#feature-request" title="Permalink">¶</a></h2>

- Developing software takes a lot of time. Don't expect anyone to work for free.
- As mentioned above, the number of features is intentionally limited. Nobody likes bloatware.
- Improving existing features is more important than adding new ones.

<h2 id="golang">Why choose Golang as the programming language? <a class="anchor" href="#golang" title="Permalink">¶</a></h2>

[Go](https://golang.org/) is probably the best choice for self-hosted software:

- Go is a simple programming language.
- Running code concurrently is built into the language.
- It's faster than scripting languages like PHP or Python.
- The final application is a statically compiled binary without any dependencies.
- You just need to drop the executable on your server to deploy the application.
- You don't have to worry about what version of PHP/Python is installed on your machine.
- Packaging the software using RPM/Debian/Docker is straightforward.
- No dependency hell.

<h2 id="postgresql">Why PostgreSQL? <a class="anchor" href="#postgresql" title="Permalink">¶</a></h2>

Miniflux is compatible only with [PostgreSQL](https://www.postgresql.org/).

- Supporting multiple databases increases the complexity of the software.
- We do not have the resources to test the software with all major versions of MySQL, MariaDB, SQLite, and others.
- [SQLite](https://www.sqlite.org/) is excellent, but it has limited support for `ALTER TABLE`, and the [Golang driver](https://github.com/mattn/go-sqlite3) requires [CGO](https://golang.org/cmd/cgo/) and [GCC](https://gcc.gnu.org/). Miniflux 2 was initially designed as a [Twelve-Factor App](https://12factor.net/) to run in ephemeral containers.
- PostgreSQL is powerful, rock solid, and a great independent open-source software.
- Miniflux does not use ORMs because they abstract away powerful features provided by the database.
- Miniflux uses `hstore/jsonb/inet` data types, window functions, full-text search, and handles user timezones with PostgreSQL.

<h2 id="js-framework">Why no Javascript framework? <a class="anchor" href="#js-framework" title="Permalink">¶</a></h2>

Miniflux uses Javascript only where it's necessary.

- Server-side template rendering is very simple and fast enough for this kind of application.
- Using Javascript frameworks increases complexity.
- The Javascript ecosystem is constantly changing; sticking to the standard is more sustainable.
- Having too many dependencies is challenging to maintain in the long term and can introduce security vulnerabilities.

<h2 id="es6">Why only ECMAScript 6? <a class="anchor" href="#es6" title="Permalink">¶</a></h2>

Miniflux uses [ECMAScript 6](https://en.wikipedia.org/wiki/ECMAScript#6th_Edition_-_ECMAScript_2015) and the Fetch API.

- All modern browsers support ES6 nowadays.
- Only Internet Explorer doesn't support ES6, but who cares?
- Using a Javascript transpiler introduces another set of unnecessary dependencies.

<h2 id="mobile-app">Why is there no mobile application? <a class="anchor" href="#mobile-app" title="Permalink">¶</a></h2>

Using the web UI on your smartphone is not so bad:

- Miniflux is a [progressive web app](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps).
- The layout adapts to the screen size (responsive design).
- You can add the application to the home screen like any native application.
- You can swipe entries horizontally to change their status.
- The web browser is a pretty good sandbox; the application cannot access the data stored on your phone.
- It's cross-platform: works on iOS and Android.

The development of mobile clients is left to the open-source community:

- Developing a native mobile application for each platform (iOS and Android) and different devices (smartphones and tablets) takes a lot of work.
- The main developer of Miniflux is not a mobile application developer.
- You have to pay a fee to publish your app on the store, even if your app doesn't make any money.
