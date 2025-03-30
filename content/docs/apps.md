---
title: Third-Party Applications
url: /docs/apps.html
---

Table of Contents

- [FeedMe](https://play.google.com/store/apps/details?id=com.seazon.feedme&hl=en) (Android / Free / Proprietary)
- [Fluent Reader](https://hyliu.me/fluent-reader/) (All platforms / FOSS except iPhone and Android non-free versions)
- [Fluxjs](#fluxjs) (Web frontend)
- [FluxNews](#fluxnews) (Android / FOSS)
- [FocusReader](https://play.google.com/store/apps/details?id=allen.town.focus.reader) (Android / Freemium / Proprietary)
- [Lire](#lire) (iOS/macOS / Paid / Proprietary)
- [Microflux](#microflux) (Android)
- [Miniflutt](#miniflutt) (Android)
- [Miniflux-ai](#miniflux-ai) (Adds AI summaries, translations, and AI news based on RSS content)
- [NetNewsWire](https://netnewswire.com/) (iOS/macOS / FOSS)
- [Newsboat](https://github.com/newsboat/newsboat) (Text console / FOSS)
- [NewsFlash](https://gitlab.com/news-flash/news_flash_gtk) (Linux / FOSS)
- [ReactFlux](#ReactFlux) (Web frontend)
- [Read You](https://github.com/Ashinch/ReadYou) (Android / FOSS)
- [ReadKit](https://readkit.app/) (iOS/macOS / Freemium / Proprietary)
- [Reeder Classic](http://reederapp.com/classic) (iOS/macOS / Paid / Proprietary)
- [Reminiflux](#reminiflux) (Web frontend)
- [RSS Guard](https://github.com/martinrotter/rssguard) (Windows, Linux, BSD, OS/2, or macOS / FOSS)
- [Telegram](https://telegram.org/)
- [Unread](https://www.goldenhillsoftware.com/unread/) (iOS/macOS / Freemium / Proprietary)

<h2 id="fluxjs">Fluxjs <a class="anchor" href="#fluxjs" title="Permalink">¶</a></h2>

Fluxjs is a responsive design frontend. You can browse your RSS feeds from your mobile or desktop.

Links: [GitHub](https://github.com/PascalNoisette/fluxjs)

<h2 id="fluxnews">FluxNews <a class="anchor" href="#fluxnews" title="Permalink">¶</a></h2>

A simple newsreader for the Miniflux backend. This newsreader syncs with the Miniflux server API.

It supports light and dark modes, marks articles as read on scroll-over, and opens articles in a preferred installed app.

Links: [GitHub](https://github.com/KevinCFechtel/FluxNews)

<h2 id="lire">Lire <a class="anchor" href="#lire" title="Permalink">¶</a></h2>

Lire can sync with Miniflux. It uses both the Fever API and the Miniflux API.

![Screenshot](https://lireapp.com/iOS.png)

### Configuration

1. Enable the Fever integration in Miniflux and define a Fever username and password (Go to **Settings > Integrations**).

![Fever API](/images/lire_fever_settings.png)

2. Create a new Miniflux API Key in **Settings > API Keys**.

![Miniflux API Key](/images/lire_miniflux_api_key.png)

3. Enter the connection information created above in Lire settings (select Miniflux in the list of self-hosted sync services).

![Lire Settings](/images/lire_settings.png)

- Make sure to copy and paste the entire API Key (double-check for typos).
- Do not use `/v1/` or `/fever/` in the Miniflux URL. For example, the URL is `https://reader.miniflux.app` for the hosted version of Miniflux.

Links: [Official website](https://lireapp.com/), [iOS App Store](http://itunes.apple.com/app/lire/id1531976425?ls=1&mt=8), [Mac App Store](https://apps.apple.com/us/app/lire/id1482527526?ls=1&mt=12)

<h2 id="microflux">Microflux <a class="anchor" href="#microflux" title="Permalink">¶</a></h2>

This reader has features that cannot be provided to form a conventional web page.

Features:

- Offline persistence and sync
- Multiple accounts can be stored
- Material design and animations
- Dark and light modes
- Articles display an image preview
- Notifications

Links: [GitHub](https://github.com/ConstantinCezarBegu/Microflux)

<h2 id="miniflutt">Miniflutt <a class="anchor" href="#miniflutt" title="Permalink">¶</a></h2>

Features:

- Online reading
- Audio and video playback
- Article search
- Share article
- Open article in external browser
- Dark theme available on night mode
- Download most common files (documents, images and videos)
- Articles are grouped by categories / feeds.
- All articles from a category / feed can be marked as read.
- Fetch read and/or unread articles
- Set favorites
- Feed and category management

Links: [Google Play](https://play.google.com/store/apps/details?id=be.martinelli.miniflutt),
[GitHub](https://github.com/DocMarty84/miniflutt)

<h2 id="miniflux-ai">Miniflux-ai <a class="anchor" href="#miniflux-ai" title="Permalink">¶</a></h2>

This project integrates with Miniflux to fetch RSS feed content via API or webhook.
It then utilizes large language models (e.g., Ollama, ChatGPT, LLaMA, Gemini) to generate summaries, translations, and AI-driven news insights.

Links: [GitHub](https://github.com/Qetesh/miniflux-ai)

<h2 id="ReactFlux">ReactFlux <a class="anchor" href="#ReactFlux" title="Permalink">¶</a></h2>

ReactFlux is a third-party web frontend for Miniflux, aimed at providing a more user-friendly reading experience.

Key features include:

- Modern interface design
- Responsive layout
- Support for dark mode and custom themes
- Article search with Google-like syntax
- Feed and group management
- Keyboard shortcuts
- Automatically mark articles as read when scrolling
- Batch update the host of filtered subscription URLs (useful for replacing RSSHub instances)
- Batch refresh recently errored subscriptions
- De-duplicate articles by hash, title, or URL when loading the list
- i18n supports (include: English / Español / 简体中文)
- Other features waiting for you to discover...

Links: [GitHub](https://github.com/electh/ReactFlux)

<h2 id="reminiflux">Reminiflux <a class="anchor" href="#reminiflux" title="Permalink">¶</a></h2>

Reminiflux is an alternative web frontend. It offers a look and feel which is more similar to
Google Reader and TT-RSS with a 3-paned display showing the list of feeds, items and an article.

Links: [GitHub](https://github.com/reminiflux/reminiflux)
