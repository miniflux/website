---
title: User Interface Usage
url: /docs/ui.html
---
<h2 id="list-view">List View <a class="anchor" href="#list-view" title="Permalink">¶</a></h2>

![List View](/images/list_view.png)

- **Save**: Send an article to third-party services if enabled (Pinboard, Instapaper, etc.).
- **Star/Unstar**: Add/Remove an entry to/from bookmarks.
- **Original**: Open the original entry link in a new tab.

<p class="info">
Since Miniflux 2.0.7, the "Save" link will be shown only if at least one integration is configured.
</p>

<h2 id="article-view">Article View <a class="anchor" href="#article-view" title="Permalink">¶</a></h2>

![Article View](/images/entry_view.png)

- **Fetch original content**: Download the original web page and try to find relevant content.

<h2 id="edit-feed">Edit Feed <a class="anchor" href="#edit-feed" title="Permalink">¶</a></h2>

![Edit Feed](/images/edit_feed.png)

- **Set Cookies**: HTTP Cookies to add to the request, e.g., `name0=value0; name1=value1`.
- **Scraper Rules**: CSS selectors to use when fetching the original web page (Use Readability if nothing is provided).
- **Rewrite Rules**: Name of the rewrite rules to use to alter item content.
- **Fetch original content**: Always download original articles (consumes more resources).
