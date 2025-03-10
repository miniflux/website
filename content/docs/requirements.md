---
title: Requirements
description: List of requirements to install and use Miniflux
url: docs/requirements.html
---
<h2 id="operating-systems">Operating Systems <a class="anchor" href="#operating-systems" title="Permalink">¶</a></h2>

- Linux (Alpine Linux, Debian, RHEL, etc.)
- [Darwin](https://github.com/golang/go/wiki/Darwin) (macOS)
- [FreeBSD](https://github.com/golang/go/wiki/FreeBSD)
- [OpenBSD](https://github.com/golang/go/wiki/OpenBSD)
- Windows

<p class="info">All operating systems supported by Golang should work but Miniflux is mainly tested with Linux.</p>

<h2 id="database">Database <a class="anchor" href="#database" title="Permalink">¶</a></h2>

Only PostgreSQL >= 9.5 is supported

<h2 id="web-browsers">Web Browsers <a class="anchor" href="#web-browsers" title="Permalink">¶</a></h2>

A browser compatible with [ECMAScript 11](https://en.wikipedia.org/wiki/ECMAScript_version_history#11th_Edition_%E2%80%93_ECMAScript_2020) is required.

Miniflux is tested only with the most popular web browsers:

- Mozilla Firefox
- Chrome
- Safari
- Microsoft Edge

Keep in mind that testing Miniflux with all possible browsers is not feasible due to limited time and resources. WebKit, Chromium, and Firefox forks should work, but this is not guaranteed.

<p class="warning">Internet Explorer and the Kindle browser are not supported.</p>
