title: Features
description: List of Features
template: page
uri: features.html
---

<h2 id="reader">Reader <a class="anchor" href="#reader" title="Permalink">¶</a></h2>

- Supported feed formats: Atom, RSS 1.0/2.0, RDF and JSON
- [OPML](https://en.wikipedia.org/wiki/OPML) import/export
- Support multiple enclosures/attachments (Podcasts, videos, music, and images)
- Play videos from YouTube channels directly inside Miniflux
- Categories
- Bookmarks
- Fetch website icons (favicons)
- Save articles to third-party services
- Available in Chinese, Dutch, English, French, German, Italian, Polish, Russian, and Spanish

<h2 id="privacy">Privacy <a class="anchor" href="#privacy" title="Permalink">¶</a></h2>

- Remove pixel trackers
- Fetch original links when the feed is coming from FeedBurner
- Open external links with the attributes `rel="noopener noreferrer" referrerpolicy="no-referrer"`
- Image proxy to avoid mixed content warnings with HTTPS
- Play Youtube videos by using the domain `youtube-nocookie.com`
- Block any external Javascript code to avoid tracking

<h2 id="content-manipulation">Content Manipulation <a class="anchor" href="#content-manipulation" title="Permalink">¶</a></h2>

- Fetch original article and returns only relevant contents (Readability parser)
- Custom scraper rules based on <abbr title="Cascading Style Sheets">CSS</abbr> selectors
- Custom rewriting rules
- Override default user agent to bypass websites restrictions

<h2 id="ui">User Interface <a class="anchor" href="#ui" title="Permalink">¶</a></h2>

- Stylesheet optimized for readability
- Responsive design (works on desktop, tablet, and mobile devices)
- No fancy user interface
- Doesn't require to download an application from the App/Play Store
- You could add Miniflux to the home screen
- Keyboard shortcuts
- Touch events on mobile devices
- Themes: Default (light), Black (Dark mode) and Sans-Serif

<h2 id="integration">Integration <a class="anchor" href="#integration" title="Permalink">¶</a></h2>

- Send articles to [Pinboard](https://pinboard.in/), [Instapaper](https://www.instapaper.com/), [Pocket](https://getpocket.com/), [Wallabag](https://www.wallabag.org/), or Nunux Keeper
- Bookmarklet to subscribe to a website directly from any web browser
- Use existing mobile applications to read your feeds by using the Fever API
- REST API with clients written in [Go](https://github.com/miniflux/miniflux/tree/master/client) and [Python](https://github.com/miniflux/python-client)

<h2 id="auth">Authentication <a class="anchor" href="#auth" title="Permalink">¶</a></h2>

- Username/password
- Google (OAuth2)

<h2 id="tech">Technical Stuff <a class="anchor" href="#tech" title="Permalink">¶</a></h2>

- Self-hosted
- Written in [Go (Golang)](https://golang.org/)
- Compatible only with [Postgresql](https://www.postgresql.org/)
- There is no dependency, only a **static binary**
- Automatic HTTPS configuration with Let's Encrypt
- Use your own <abbr title="Secure Sockets Layer">SSL</abbr> certificate
- Supports [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2) if TLS is configured
- Feeds are updated in the background by an internal scheduler
- External content is sanitized before being displayed
- Uses a [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) that allows only application Javascript and block inline code and styles
- Works only in modern browsers
- Follows the [Twelve-Factor App](https://12factor.net/) principle
