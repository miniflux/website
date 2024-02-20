---
title: Fever API
url: /docs/fever.html
---

Miniflux implements the [Fever API](https://feedafever.com/api) in addition to its own [REST API](api.html). The
Fever API allows you to use existing mobile applications to read your
feeds instead of the web user interface.

To activate the Fever API, go to the integration section and choose a username/password.

![Fever API](/images/fever.png)

## Compatible Apps

- [Reeder](http://reederapp.com/) (iOS/macOS)
- [Unread](https://www.goldenhillsoftware.com/unread/) (iOS)
- [FeedMe](https://play.google.com/store/apps/details?id=com.seazon.feedme&hl=en) (Android)
- [NewsFlash](https://gitlab.com/news-flash/news_flash_gtk) (Linux)
- [Fluent Reader](https://hyliu.me/fluent-reader/) (Windows and Mac)
- [Lire](https://lireapp.com/) (iOS/macOS) - Also requires a Miniflux API Key

## Notes

- Saving an entry will add a new bookmark and save the article
- Only the JSON format is supported
- Refreshing feeds is not possible with Reeder because no user information is sent
- Links, sparks, and kindlings are not supported
