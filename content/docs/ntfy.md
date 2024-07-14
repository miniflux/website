---
title: Ntfy Integration
url: /docs/ntfy.html
---

[ntfy](https://ntfy.sh/) (pronounced notify) is a simple HTTP-based pub-sub notification service. It allows you to send notifications to your phone or desktop via scripts from any computer, and/or using a REST API. It's a highly flexible and completely free software.

- [Official website](https://ntfy.sh/)
- [Git repository](https://github.com/binwiederhier/ntfy)

Miniflux can send new entries to any **ntfy** server. By doing so, you can receive push notifications whenever a new article is published.

To receive notifications on your phone, install the app. Once installed, open it and subscribe to a topic of your choosing.

Note that topic names are public, so it's wise to choose something that cannot be easily guessed.
You can also protect your topic with a username/password or an API token (see [ntfy documentation](https://docs.ntfy.sh/config/#access-control)).

![Integration Settings](/images/ntfy_integration_settings.png)

Optionally, you can add an icon (compatible with Android only). Feel free to use a Miniflux icon served by GitHub Pages, for example: `https://miniflux.app/logo/original/favicon-32-bg-white.png`.

To avoid unnecessary requests, make sure to enable push notifications explicitly for each feed.

Go to the feed settings page and enable **ntfy**.

![Feed Settings](/images/ntfy_feed_settings.png)

You can also choose a different priority for each feed. Priorities define how urgently your phone notifies you. Refer to the [official ntfy documentation for more information](https://docs.ntfy.sh/publish/#message-priority).