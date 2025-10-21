---
title: RSS-Bridge
url: /docs/rss_bridge.html
---

[RSS-Bridge](https://rss-bridge.org/) is an application that generates RSS and
Atom feeds for websites that don't provide them natively. It acts as a bridge
between services without feeds and your RSS reader, allowing you to follow
content from social media platforms, forums, and other websites through
standard feed formats.

Miniflux can use RSS-Bridge to automatically create feed links when it cannot
find a feed at the provided URL. When you add a subscription and Miniflux
doesn't detect a native feed, it can attempt to generate one through your
configured RSS-Bridge instance.

## Configuring RSS-Bridge integration

To activate RSS-Bridge connection go to **Settings → Integrations →
RSS-Bridge** section and provide link to your server instance and credential
details.

## Authentication

RSS-Bridge supports authentication to restrict access to your instance.
Miniflux can work with authenticated RSS-Bridge instances in two ways:

### Method 1: Token Authentication

1. Configure authentication in your RSS-Bridge instance by setting up a token
   in the RSS-Bridge configuration file.
1. In Miniflux, enter your RSS-Bridge authentication token in integration
   settings.

### Method 2: HTTP Basic Authentication

1. Configure authentication in your RSS-Bridge instance by username and
   password in the RSS-Bridge configuration file.
1. In Miniflux, embed these credentials directly in RSS-Bridge server URL:
   `https://username:password@rss-bridge-example`.

## Notes

* Miniflux creates the RSS-Bridge URL when the feed is
  initially generated. This link includes the instance address and credentials.
  If you need to change these settings later, you must update the feed URL
  directly, as changes in integration settings won't affect existing feeds.
* Miniflux relies on RSS-Bridge's internal functionality to
  detect whether it can generate a feed URL. Bridge configurations may not
  include all possible URL patterns, so you might need to create some feeds
  manually in RSS-Bridge.
* Bridges can include additional settings to customize
  their content. You may want to create feeds manually in RSS-Bridge to modify
  default settings and fine-tune the output.

