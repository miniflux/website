---
title: Features
description: List of Features
url: features.html
---

<h2 id="reader">Reader <a class="anchor" href="#reader" title="Permalink">¶</a></h2>

- Supported feed formats: Atom 0.3/1.0, RSS 1.0/2.0, RDF and JSON
- [OPML](https://en.wikipedia.org/wiki/OPML) file import/export and URL import
- Support multiple enclosures/attachments (Podcasts, videos, music, and images)
- Play videos from YouTube channels directly inside Miniflux
- Categories
- Bookmarks
- Fetch website icons (favicons)
- Save articles to third-party services
- Full-text search (Thanks to Postgres)
- Available in Portuguese (Brazilian), Chinese (Simplified and Traditional), Dutch, English (US), Finnish, French, German, Greek, Italian, Japanese, Polish, Russian, Spanish, and Turkish

<h2 id="privacy">Privacy <a class="anchor" href="#privacy" title="Permalink">¶</a></h2>

- Remove pixel trackers
- Fetch original links when the feed is coming from FeedBurner
- Open external links with the attributes `rel="noopener noreferrer" referrerpolicy="no-referrer"`
- Use the HTTP header `Referrer-Policy: no-referrer`
- Image proxy to avoid mixed content warnings with HTTPS
- Play Youtube videos by using the domain `youtube-nocookie.com`
- Supports alternative YouTube video players like [Invidious](https://invidio.us)
- Block external Javascript code to avoid tracking

<h2 id="content-manipulation">Content Manipulation <a class="anchor" href="#content-manipulation" title="Permalink">¶</a></h2>

- Fetch original article and returns only relevant contents (Readability parser)
- Custom scraper rules based on <abbr title="Cascading Style Sheets">CSS</abbr> selectors
- Custom rewriting rules
- Regex filter to allow or block articles
- Override default user agent to bypass websites restrictions
- Option to allow self-signed or invalid certificates (disabled by default)
- Scrape YouTube's website to get video duration as read time (disabled by default)

<h2 id="ui">User Interface <a class="anchor" href="#ui" title="Permalink">¶</a></h2>

- Stylesheet optimized for readability
- Responsive design (works on desktop, tablet, and mobile devices)
- No fancy user interface
- Doesn't require to download an application from Apple/Google Store
- You could add Miniflux to the home screen
- Keyboard shortcuts
- Touch events on mobile devices
- Themes:
    - Light (Sans-Serif)
    - Light (Serif)
    - Dark (Sans-Serif)
    - Dark (Serif)
    - System (Sans-Serif) - Switch between Dark and Light themes according to system preferences
    - System (Serif)

<h2 id="integration">Integration <a class="anchor" href="#integration" title="Permalink">¶</a></h2>

- Send articles to Telegram, [Pinboard](https://pinboard.in/), [Instapaper](https://www.instapaper.com/), [Pocket](https://getpocket.com/), [Wallabag](https://www.wallabag.org/), [Linkding](https://github.com/sissbruecker/linkding), [Espial](https://github.com/jonschoning/espial), or [Nunux Keeper](https://keeper.nunux.org/)
- Bookmarklet to subscribe to a website directly from any web browser
- Use existing mobile applications to read your feeds by using the Fever or Google Reader API
- REST API with clients written in [Go](https://github.com/miniflux/v2/tree/master/client) and [Python](https://github.com/miniflux/python-client)

<h2 id="auth">Authentication <a class="anchor" href="#auth" title="Permalink">¶</a></h2>

- Username/password
- Google (OAuth2)
- OpenID Connect

<h2 id="tech">Technical Stuff <a class="anchor" href="#tech" title="Permalink">¶</a></h2>

- Self-hosted
- Written in [Go (Golang)](https://golang.org/)
- Compatible only with [Postgresql](https://www.postgresql.org/)
- There is no dependency, only a **static binary**
- All static files are bundled into the application binary using Golang embed package
- Supports Systemd sd_notify protocol
- Automatic HTTPS configuration with Let's Encrypt
- Use your own <abbr title="Secure Sockets Layer">SSL</abbr> certificate
- Supports [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2) if TLS is configured
- Feeds are updated in the background by an internal scheduler
- External content is sanitized before being displayed
- Uses a [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) that allows only application Javascript and block inline code and styles
- Support **native** lazy loading for images and iframes
- Works only in modern browsers
- Follows the [Twelve-Factor App](https://12factor.net/) principle
- Official Debian/RPM packages and pre-built binaries
- Docker image is published automatically to Docker Hub and GitHub Registry (supports ARM architectures)
