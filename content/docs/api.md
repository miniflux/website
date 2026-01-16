---
title: API Reference
url: /docs/api.html
---

Table of Contents:

- [Authentication](#authentication)
- [Clients](#clients)
    - [Golang Client](#go-client)
    - [Python Client](#python-client)
- [API Endpoints](#endpoints)
    - [Status Codes](#status-codes)
    - [Error Response](#error-response)
    - [Discover Subscriptions](#endpoint-discover)
    - [Flush History](#endpoint-flush-history)
    - [Get Feeds](#endpoint-get-feeds)
    - [Get Category Feeds](#endpoint-get-category-feeds)
    - [Get Feed](#endpoint-get-feed)
    - [Get Feed Icon by Feed ID](#endpoint-get-feed-icon-by-feed-id)
    - [Get Feed Icon by Icon ID](#endpoint-get-feed-icon-by-icon-id)
    - [Mark Feed Entries as Read](#endpoint-mark-feed-entries-as-read)
    - [Create Feed](#endpoint-create-feed)
    - [Update Feed](#endpoint-update-feed)
    - [Refresh Feed](#endpoint-refresh-feed)
    - [Refresh all Feeds](#endpoint-refresh-all-feeds)
    - [Remove Feed](#endpoint-remove-feed)
    - [Get Feed Entry](#endpoint-get-feed-entry)
    - [Get Entry](#endpoint-get-entry)
    - [Import Entry](#endpoint-import-entry)
    - [Update Entry](#endpoint-update-entry)
    - [Save entry to third-party services](#endpoint-save-entry)
    - [Fetch original article](#endpoint-fetch-content)
    - [Get Feed Entries](#endpoint-get-feed-entries)
    - [Get Category Entries](#endpoint-get-category-entries)
    - [Get Entries](#endpoint-get-entries)
    - [Update Entries status](#endpoint-update-entries)
    - [Toggle Entry Bookmark](#endpoint-toggle-bookmark)
    - [Get Enclosure](#endpoint-get-enclosure)
    - [Update Enclosure](#endpoint-update-enclosure)
    - [Get Categories](#endpoint-get-categories)
    - [Create Category](#endpoint-create-category)
    - [Update Category](#endpoint-update-category)
    - [Refresh Category Feeds](#endpoint-refresh-category)
    - [Delete Category](#endpoint-delete-category)
    - [Mark Category Entries as Read](#endpoint-mark-category-entries-as-read)
    - [OPML Export](#endpoint-export)
    - [OPML Import](#endpoint-import)
    - [Create User](#endpoint-create-user)
    - [Update User](#endpoint-update-user)
    - [Get Current User](#endpoint-me)
    - [Get User](#endpoint-get-user)
    - [Get Users](#endpoint-get-users)
    - [Delete User](#endpoint-delete-user)
    - [Integrations Status](#integrations-status)
    - [Mark User Entries as Read](#endpoint-mark-user-entries-as-read)
    - [Fetch unread and read counters](#endpoint-counters)
    - [Get API Keys](#endpoint-get-api-keys)
    - [Create API Key](#endpoint-create-api-key)
    - [Delete API Key](#endpoint-delete-api-key)
    - [Healthcheck](#endpoint-healthcheck)
    - [Liveness](#endpoint-liveness)
    - [Readiness](#endpoint-readiness)
    - [Application version](#deprecated-endpoint-version) (deprecated)
    - [Application version and build information](#endpoint-version)

<h2 id="authentication">Authentication <a class="anchor" href="#authentication" title="Permalink">¶</a></h2>

The API supports two authentication mechanisms:

- HTTP Basic authentication with the account username/password.
- Per-application API keys (since version 2.0.21) -> **preferred method**.

To generate a new API token, got to "Settings > API Keys > Create a new API key".

### HTTP Basic Authentication Example

```bash
curl -u your-miniflux-username https://miniflux.example.org/v1/me
```

### API Token Authentication Example

Miniflux uses the HTTP header `X-Auth-Token` for API token authentication.

```bash
curl -H "X-Auth-Token: your-token" https://miniflux.example.org/v1/me
```

<h2 id="clients">Clients <a class="anchor" href="#clients" title="Permalink">¶</a></h2>

There are two official API clients, one written in Go and another one
written in Python.

<h3 id="go-client">Golang Client <a class="anchor" href="#go-client" title="Permalink">¶</a></h3>

- Repository: <https://github.com/miniflux/v2/tree/main/client>
- Reference: <https://pkg.go.dev/miniflux.app/v2/client>

Installation:

```bash
go get -u miniflux.app/v2/client
```

Usage Example:

```go
package main

import (
    "fmt"

    miniflux "miniflux.app/v2/client"
)

func main() {
    // Authentication using username/password.
    client := miniflux.NewClient("https://miniflux.example.org", "admin", "secret")

    // Authentication using API token.
    client := miniflux.NewClient("https://miniflux.example.org", "My secret token")

    // Fetch all feeds.
    feeds, err := client.Feeds()
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(feeds)
}
```

<h3 id="python-client">Python Client <a class="anchor" href="#python-client" title="Permalink">¶</a></h3>

- Repository: <https://github.com/miniflux/python-client>
- PyPi: <https://pypi.org/project/miniflux/>

Installation:

```bash
pip install miniflux
```

Usage example:

```python
import miniflux

# Authentication using username/password
client = miniflux.Client("https://miniflux.example.org", "my_username", "my_secret_password")

# Authentication using an API token
client = miniflux.Client("https://miniflux.example.org", api_key="My Secret Token")

# Get all feeds
feeds = client.get_feeds()

# Refresh a feed
client.refresh_feed(123)

# Discover subscriptions from a website
subscriptions = client.discover("https://example.org")

# Create a new feed, with a personalized user agent and with the crawler enabled
feed_id = client.create_feed("http://example.org/feed.xml", 42, crawler=True, user_agent="GoogleBot")

# Fetch 10 starred entries
entries = client.get_entries(starred=True, limit=10)

# Fetch last 5 feed entries
feed_entries = client.get_feed_entries(123, direction='desc', order='published_at', limit=5)

# Update a feed category
client.update_feed(123, category_id=456)
```

<h2 id="endpoints">API Endpoints <a class="anchor" href="#endpoints" title="Permalink">¶</a></h2>

<h3 id="status-codes">Status Codes <a class="anchor" href="#status-codes" title="Permalink">¶</a></h3>

- `200`: Everything is OK
- `201`: Resource created/modified
- `204`: Resource removed/modified
- `400`: Bad request
- `401`: Unauthorized (bad username/password)
- `403`: Forbidden (access not allowed)
- `500`: Internal server error

<h3 id="error-response">Error Response <a class="anchor" href="#error-response" title="Permalink">¶</a></h3>

``` json
{
    "error_message": "Some error"
}
```

<h3 id="endpoint-discover">Discover Subscriptions <a class="anchor" href="#endpoint-discover" title="Permalink">¶</a></h3>

Request:

    POST /v1/discover
    Content-Type: application/json

    {
        "url": "http://example.org"
    }

Response:

```json
[
    {
        "url": "http://example.org/feed.atom",
        "title": "Atom Feed",
        "type": "atom"
    },
    {
        "url": "http://example.org/feed.rss",
        "title": "RSS Feed",
        "type": "rss"
    }
]
```

Optional fields:

- `username`: Feed username (string)
- `password`: Feed password (string)
- `user_agent`: Custom user agent (string)
- `fetch_via_proxy` (boolean)


<h3 id="endpoint-flush-history">Flush History <a class="anchor" href="#endpoint-flush-history" title="Permalink">¶</a></h3>

Request:

    PUT /v1/flush-history

Note that `DELETE` is also supported.

Returns a `202 Accepted` status code for success.

<div class="info">
This API endpoint is available since Miniflux v2.0.49.
</div>

<h3 id="endpoint-get-feeds">Get Feeds <a class="anchor" href="#endpoint-get-feeds" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds

Response:

```json
[
    {
        "id": 42,
        "user_id": 123,
        "title": "Example Feed",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_message": "",
        "parsing_error_count": 0,
        "scraper_rules": "",
        "rewrite_rules": "",
        "crawler": false,
        "blocklist_rules": "",
        "keeplist_rules": "",
        "user_agent": "",
        "username": "",
        "password": "",
        "disabled": false,
        "ignore_http_cache": false,
        "fetch_via_proxy": false,
        "category": {
            "id": 793,
            "user_id": 123,
            "title": "Some category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }
]
```

Notes:

- `icon` is `null` when the feed doesn't have any favicon.

<h3 id="endpoint-get-category-feeds">Get Category Feeds <a class="anchor" href="#endpoint-get-category-feeds" title="Permalink">¶</a></h3>

Request:

    GET /v1/categories/40/feeds

Response:

```json
[
    {
        "id": 42,
        "user_id": 123,
        "title": "Example Feed",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_message": "",
        "parsing_error_count": 0,
        "scraper_rules": "",
        "rewrite_rules": "",
        "crawler": false,
        "blocklist_rules": "",
        "keeplist_rules": "",
        "user_agent": "",
        "username": "",
        "password": "",
        "disabled": false,
        "ignore_http_cache": false,
        "fetch_via_proxy": false,
        "category": {
            "id": 40,
            "user_id": 123,
            "title": "Some category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }
]
```

<div class="info">
This API endpoint is available since Miniflux v2.0.29.
</div>

<h3 id="endpoint-get-feed">Get Feed <a class="anchor" href="#endpoint-get-feed" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/42

Response:

```json
{
    "id": 42,
    "user_id": 123,
    "title": "Example Feed",
    "site_url": "http://example.org",
    "feed_url": "http://example.org/feed.atom",
    "checked_at": "2017-12-22T21:06:03.133839-05:00",
    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
    "parsing_error_message": "",
    "parsing_error_count": 0,
    "scraper_rules": "",
    "rewrite_rules": "",
    "crawler": false,
    "blocklist_rules": "",
    "keeplist_rules": "",
    "user_agent": "",
    "username": "",
    "password": "",
    "disabled": false,
    "ignore_http_cache": false,
    "fetch_via_proxy": false,
    "category": {
        "id": 793,
        "user_id": 123,
        "title": "Some category"
    },
    "icon": {
        "feed_id": 42,
        "icon_id": 84
    }
}
```

Notes:

- `icon` is `null` when the feed doesn't have any favicon.

<h3 id="endpoint-get-feed-icon-by-feed-id">Get Feed Icon By Feed ID<a class="anchor" href="#endpoint-get-feed-icon-by-feed-id" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/{feedID}/icon

Response:

```json
{
    "id": 262,
    "data": "image/png;base64,iVBORw0KGgoAAA....",
    "mime_type": "image/png"
}
```

If the feed doesn't have any favicon, a 404 is returned.

<h3 id="endpoint-get-feed-icon-by-icon-id">Get Feed Icon By Icon ID<a class="anchor" href="#endpoint-get-feed-icon-by-icon-id" title="Permalink">¶</a></h3>

Request:

    GET /v1/icons/{iconID}

Response:

```json
{
    "id": 262,
    "data": "image/png;base64,iVBORw0KGgoAAA....",
    "mime_type": "image/png"
}
```

<div class="info">
This API endpoint is available since Miniflux v2.0.49.
</div>

<h3 id="endpoint-create-feed">Create Feed <a class="anchor" href="#endpoint-create-feed" title="Permalink">¶</a></h3>

Request:

    POST /v1/feeds
    Content-Type: application/json

    {
        "feed_url": "http://example.org/feed.atom",
        "category_id": 22
    }

Response:

```json
{
    "feed_id": 262,
}
```

Required fields:

- `feed_url`: Feed URL (string)
- `category_id`: Category ID (int, optional since Miniflux >= 2.0.49)

Optional fields:

- `username`: Feed username (string)
- `password`: Feed password (string)
- `crawler`: Enable/Disable scraper (boolean)
- `user_agent`: Custom user agent for the feed (string)
- `scraper_rules`: List of scraper rules (string) - Miniflux >= 2.0.19
- `rewrite_rules`: List of rewrite rules (string) - Miniflux >= 2.0.19
- `blocklist_rules` (string) - Miniflux >= 2.0.27
- `keeplist_rules` (string) - Miniflux >= 2.0.27
- `disabled` (boolean) - Miniflux >= 2.0.27
- `ignore_http_cache` (boolean) - Miniflux >= 2.0.27
- `fetch_via_proxy` (boolean) - Miniflux >= 2.0.27

<h3 id="endpoint-update-feed">Update Feed <a class="anchor" href="#endpoint-update-feed" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/42
    Content-Type: application/json

    {
        "title": "New Feed Title",
        "category_id": 22
    }

Response:

```json
{
    "id": 42,
    "user_id": 123,
    "title": "New Feed Title",
    "site_url": "http://example.org",
    "feed_url": "http://example.org/feed.atom",
    "checked_at": "2017-12-22T21:06:03.133839-05:00",
    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
    "parsing_error_message": "",
    "parsing_error_count": 0,
    "scraper_rules": "",
    "rewrite_rules": "",
    "crawler": false,
    "blocklist_rules": "",
    "keeplist_rules": "",
    "user_agent": "",
    "username": "",
    "password": "",
    "disabled": false,
    "ignore_http_cache": false,
    "fetch_via_proxy": false,
    "category": {
        "id": 22,
        "user_id": 123,
        "title": "Another category"
    },
    "icon": {
        "feed_id": 42,
        "icon_id": 84
    }
}
```

Available fields:

- `feed_url` (string)
- `site_url` (string)
- `title` (string)
- `category_id` (int)
- `scraper_rules` (string)
- `rewrite_rules` (string)
- `blocklist_rules` (string)
- `keeplist_rules` (string)
- `crawler` (boolean)
- `user_agent`: Custom user agent for the feed (string)
- `username` (string)
- `password` (string)
- `disabled` (boolean)
- `ignore_http_cache` (boolean)
- `fetch_via_proxy` (boolean)

<h3 id="endpoint-refresh-feed">Refresh Feed <a class="anchor" href="#endpoint-refresh-feed" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/42/refresh

<ul>
    <li>Returns <code>204</code> status code for success.</li>
    <li>This API call is synchronous and can takes hundred of milliseconds.</li>
</ul>

<h3 id="endpoint-refresh-all-feeds">Refresh all Feeds <a class="anchor" href="#endpoint-refresh-all-feeds" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/refresh

<ul>
    <li>Returns <code>204</code> status code for success.</li>
    <li>Feeds are refreshed in a background process.</li>
    <li>Available since Miniflux 2.0.21</li>
</ul>

<h3 id="endpoint-remove-feed">Remove Feed <a class="anchor" href="#endpoint-remove-feed" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/feeds/42

<h3 id="endpoint-get-feed-entry">Get Feed Entry <a class="anchor" href="#endpoint-get-feed-entry" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/42/entries/888

Response:

``` json
{
    "id": 888,
    "user_id": 123,
    "feed_id": 42,
    "title": "Entry Title",
    "url": "http://example.org/article.html",
    "comments_url": "",
    "author": "Foobar",
    "content": "<p>HTML contents</p>",
    "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
    "published_at": "2016-12-12T16:15:19Z",
    "created_at": "2016-12-27T16:15:19Z",
    "status": "unread",
    "share_code": "",
    "starred": false,
    "reading_time": 1,
    "enclosures": null,
    "feed": {
        "id": 42,
        "user_id": 123,
        "title": "New Feed Title",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_message": "",
        "parsing_error_count": 0,
        "scraper_rules": "",
        "rewrite_rules": "",
        "crawler": false,
        "blocklist_rules": "",
        "keeplist_rules": "",
        "user_agent": "",
        "username": "",
        "password": "",
        "disabled": false,
        "ignore_http_cache": false,
        "fetch_via_proxy": false,
        "category": {
            "id": 22,
            "user_id": 123,
            "title": "Another category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }
}
```

<h3 id="endpoint-get-entry">Get Entry <a class="anchor" href="#endpoint-get-entry" title="Permalink">¶</a></h3>

Request:

    GET /v1/entries/888

Response:

```json
{
    "id": 888,
    "user_id": 123,
    "feed_id": 42,
    "title": "Entry Title",
    "url": "http://example.org/article.html",
    "comments_url": "",
    "author": "Foobar",
    "content": "<p>HTML contents</p>",
    "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
    "published_at": "2016-12-12T16:15:19Z",
    "created_at": "2016-12-27T16:15:19Z",
    "status": "unread",
    "share_code": "",
    "starred": false,
    "reading_time": 1,
    "enclosures": null,
    "feed": {
        "id": 42,
        "user_id": 123,
        "title": "New Feed Title",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_message": "",
        "parsing_error_count": 0,
        "scraper_rules": "",
        "rewrite_rules": "",
        "crawler": false,
        "blocklist_rules": "",
        "keeplist_rules": "",
        "user_agent": "",
        "username": "",
        "password": "",
        "disabled": false,
        "ignore_http_cache": false,
        "fetch_via_proxy": false,
        "category": {
            "id": 22,
            "user_id": 123,
            "title": "Another category"
        },
        "icon": {
            "feed_id": 42,
            "icon_id": 84
        }
    }
}
```

<h3 id="endpoint-import-entry">Import Entry <a class="anchor" href="#endpoint-import-entry" title="Permalink">¶</a></h3>

Request:

    POST /feeds/{feedID}/entries/import
    Content-Type: application/json

    {
        "title": "Entry Title",
        "url": "http://example.org/article.html",
        "author": "Foobar",
        "content": "<p>HTML contents</p>",
        "published_at": 1736200000,
        "status": "unread",
        "starred": false,
        "tags": ["tag1", "tag2"],
        "external_id": "unique-id-123",
        "comments_url": "http://example.org/article.html#comments"
    }

Note: All fields are optional except `url`.

Response:

```json
{
    "id": 1790
}
```

Returns a `201 Created` status code when the entry is created, and a `200 OK` status code when the entry already exists.

<div class="info">
This API endpoint is available since Miniflux v2.2.16.
</div>

<h3 id="endpoint-update-entry">Update Entry <a class="anchor" href="#endpoint-update-entry" title="Permalink">¶</a></h3>

Both fields `title` and `content` are optional.

Request:

    PUT /v1/entries/{entryID}

    {
        "title": "New title",
        "content": "Some text"
    }

Response:

```json
{
  "id": 1790,
  "user_id": 1,
  "feed_id": 21,
  "status": "unread",
  "hash": "22a6795131770d9577c91c7816e7c05f78586fc82e8ad0881bce69155f63edb6",
  "title": "New title",
  "url": "https://miniflux.app/releases/1.0.1.html",
  "comments_url": "",
  "published_at": "2013-03-20T00:00:00Z",
  "created_at": "2023-10-07T03:52:50.013556Z",
  "changed_at": "2023-10-07T03:52:50.013556Z",
  "content": "Some text",
  "author": "Frédéric Guillot",
  "share_code": "",
  "starred": false,
  "reading_time": 1,
  "enclosures": [],
  "feed": {
    "id": 21,
    "user_id": 1,
    "feed_url": "https://miniflux.app/feed.xml",
    "site_url": "https://miniflux.app",
    "title": "Miniflux",
    "checked_at": "2023-10-08T23:56:44.853427Z",
    "next_check_at": "0001-01-01T00:00:00Z",
    "etag_header": "",
    "last_modified_header": "",
    "parsing_error_message": "",
    "parsing_error_count": 0,
    "scraper_rules": "",
    "rewrite_rules": "",
    "crawler": false,
    "blocklist_rules": "",
    "keeplist_rules": "",
    "urlrewrite_rules": "",
    "user_agent": "",
    "cookie": "",
    "username": "",
    "password": "",
    "disabled": false,
    "no_media_player": false,
    "ignore_http_cache": false,
    "allow_self_signed_certificates": false,
    "fetch_via_proxy": false,
    "category": {
      "id": 2,
      "title": "000",
      "user_id": 1,
      "hide_globally": false
    },
    "icon": {
      "feed_id": 21,
      "icon_id": 11
    },
    "hide_globally": false,
    "apprise_service_urls": ""
  },
  "tags": []
}
```

Returns a `201 Created` status code for success.

<div class="info">
This API endpoint is available since Miniflux v2.0.49.
</div>

<h3 id="endpoint-save-entry">Save entry to third-party services <a class="anchor" href="#endpoint-save-entry" title="Permalink">¶</a></h3>

Request:

    POST /v1/entries/{entryID}/save

Response:

Returns a `202 Accepted` status code for success.

<h3 id="endpoint-fetch-content">Fetch original article <a class="anchor" href="#endpoint-fetch-content" title="Permalink">¶</a></h3>

Request:

    GET /v1/entries/{entryID}/fetch-content?update_content=true

Available fields:

- `update_content`: true or false. (default false). Whether to replace title and content in database

Response:

```json
{"content": "html content"}
```

<div class="info">
This API endpoint is available since Miniflux v2.0.36.
</div>

<h3 id="endpoint-get-category-entries">Get Category Entries <a class="anchor" href="#endpoint-get-category-entries" title="Permalink">¶</a></h3>

Request:

    GET /v1/categories/22/entries?limit=1&order=id&direction=asc

Available filters:

- `status`: Entry status (read, unread or removed), this option can be repeated to filter by multiple statuses (version >= 2.0.24)
- `offset`
- `limit`
- `order`: "id", "status", "published\_at", "category\_title",
"category\_id"
- `direction`: "asc" or "desc"
- `before` (unix timestamp, available since Miniflux 2.0.9)
- `after` (unix timestamp, available since Miniflux 2.0.9)
- `published_before` (unix timestamp, available since Miniflux 2.0.49)
- `published_after` (unix timestamp, available since Miniflux 2.0.49)
- `changed_before` (unix timestamp, available since Miniflux 2.0.49)
- `changed_after` (unix timestamp, available since Miniflux 2.0.49)
- `before_entry_id` (int64, available since Miniflux 2.0.9)
- `after_entry_id` (int64, available since Miniflux 2.0.9)
- `starred` (boolean, available since Miniflux 2.0.9)
- `search`: search query (text, available since Miniflux 2.0.10)
- `category_id`: filter by category (int, available since Miniflux 2.0.19)

Response:

```json
{
    "total": 10,
    "entries": [
        {
            "id": 888,
            "user_id": 123,
            "feed_id": 42,
            "title": "Entry Title",
            "url": "http://example.org/article.html",
            "comments_url": "",
            "author": "Foobar",
            "content": "<p>HTML contents</p>",
            "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
            "published_at": "2016-12-12T16:15:19Z",
            "created_at": "2016-12-27T16:15:19Z",
            "status": "unread",
            "share_code": "",
            "starred": false,
            "reading_time": 1,
            "enclosures": null,
            "feed": {
                "id": 42,
                "user_id": 123,
                "title": "New Feed Title",
                "site_url": "http://example.org",
                "feed_url": "http://example.org/feed.atom",
                "checked_at": "2017-12-22T21:06:03.133839-05:00",
                "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                "parsing_error_message": "",
                "parsing_error_count": 0,
                "scraper_rules": "",
                "rewrite_rules": "",
                "crawler": false,
                "blocklist_rules": "",
                "keeplist_rules": "",
                "user_agent": "",
                "username": "",
                "password": "",
                "disabled": false,
                "ignore_http_cache": false,
                "fetch_via_proxy": false,
                "category": {
                    "id": 22,
                    "user_id": 123,
                    "title": "Another category"
                },
                "icon": {
                    "feed_id": 42,
                    "icon_id": 84
                }
            }
        }
    ]
}
```

<h3 id="endpoint-get-feed-entries">Get Feed Entries <a class="anchor" href="#endpoint-get-feed-entries" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/42/entries?limit=1&order=id&direction=asc

Available filters:

- `status`: Entry status (read, unread or removed), this option can be repeated to filter by multiple statuses (version >= 2.0.24)
- `offset`
- `limit`
- `order`: "id", "status", "published\_at", "category\_title",
"category\_id"
- `direction`: "asc" or "desc"
- `before` (unix timestamp, available since Miniflux 2.0.9)
- `after` (unix timestamp, available since Miniflux 2.0.9)
- `published_before` (unix timestamp, available since Miniflux 2.0.49)
- `published_after` (unix timestamp, available since Miniflux 2.0.49)
- `changed_before` (unix timestamp, available since Miniflux 2.0.49)
- `changed_after` (unix timestamp, available since Miniflux 2.0.49)
- `before_entry_id` (int64, available since Miniflux 2.0.9)
- `after_entry_id` (int64, available since Miniflux 2.0.9)
- `starred` (boolean, available since Miniflux 2.0.9)
- `search`: search query (text, available since Miniflux 2.0.10)
- `category_id`: filter by category (int, available since Miniflux 2.0.19)

Response:

```json
{
    "total": 10,
    "entries": [
        {
            "id": 888,
            "user_id": 123,
            "feed_id": 42,
            "title": "Entry Title",
            "url": "http://example.org/article.html",
            "comments_url": "",
            "author": "Foobar",
            "content": "<p>HTML contents</p>",
            "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
            "published_at": "2016-12-12T16:15:19Z",
            "created_at": "2016-12-27T16:15:19Z",
            "status": "unread",
            "share_code": "",
            "starred": false,
            "reading_time": 1,
            "enclosures": null,
            "feed": {
                "id": 42,
                "user_id": 123,
                "title": "New Feed Title",
                "site_url": "http://example.org",
                "feed_url": "http://example.org/feed.atom",
                "checked_at": "2017-12-22T21:06:03.133839-05:00",
                "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                "parsing_error_message": "",
                "parsing_error_count": 0,
                "scraper_rules": "",
                "rewrite_rules": "",
                "crawler": false,
                "blocklist_rules": "",
                "keeplist_rules": "",
                "user_agent": "",
                "username": "",
                "password": "",
                "disabled": false,
                "ignore_http_cache": false,
                "fetch_via_proxy": false,
                "category": {
                    "id": 22,
                    "user_id": 123,
                    "title": "Another category"
                },
                "icon": {
                    "feed_id": 42,
                    "icon_id": 84
                }
            }
        }
    ]
}
```

<h3 id="endpoint-mark-feed-entries-as-read">Mark Feed Entries as Read <a class="anchor" href="#endpoint-mark-feed-entries-as-read" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/123/mark-all-as-read

Returns `204 Not Content` status code for success.

<div class="info">
This API endpoint is available since Miniflux v2.0.26.
</div>

<h3 id="endpoint-get-entries">Get Entries <a class="anchor" href="#endpoint-get-entries" title="Permalink">¶</a></h3>

Request:

    GET /v1/entries?status=unread&direction=desc

Available filters:

- `status`: Entry status (read, unread or removed), this option can be repeated to filter by multiple statuses (version >= 2.0.24)
- `offset`
- `limit`
- `order`: "id", "status", "published\_at", "category\_title",
"category\_id"
- `direction`: "asc" or "desc"
- `before` (unix timestamp, available since Miniflux 2.0.9)
- `after` (unix timestamp, available since Miniflux 2.0.9)
- `published_before` (unix timestamp, available since Miniflux 2.0.49)
- `published_after` (unix timestamp, available since Miniflux 2.0.49)
- `changed_before` (unix timestamp, available since Miniflux 2.0.49)
- `changed_after` (unix timestamp, available since Miniflux 2.0.49)
- `before_entry_id` (int64, available since Miniflux 2.0.9)
- `after_entry_id` (int64, available since Miniflux 2.0.9)
- `starred` (boolean, available since Miniflux 2.0.9)
- `search`: search query (text, available since Miniflux 2.0.10)
- `category_id`: filter by category (int, available since Miniflux 2.0.24)

Response:

```json
{
    "total": 10,
    "entries": [
        {
            "id": 888,
            "user_id": 123,
            "feed_id": 42,
            "title": "Entry Title",
            "url": "http://example.org/article.html",
            "comments_url": "",
            "author": "Foobar",
            "content": "<p>HTML contents</p>",
            "hash": "29f99e4074cdacca1766f47697d03c66070ef6a14770a1fd5a867483c207a1bb",
            "published_at": "2016-12-12T16:15:19Z",
            "created_at": "2016-12-27T16:15:19Z",
            "status": "unread",
            "share_code": "",
            "starred": false,
            "reading_time": 1,
            "enclosures": null,
            "feed": {
                "id": 42,
                "user_id": 123,
                "title": "New Feed Title",
                "site_url": "http://example.org",
                "feed_url": "http://example.org/feed.atom",
                "checked_at": "2017-12-22T21:06:03.133839-05:00",
                "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                "parsing_error_message": "",
                "parsing_error_count": 0,
                "scraper_rules": "",
                "rewrite_rules": "",
                "crawler": false,
                "blocklist_rules": "",
                "keeplist_rules": "",
                "user_agent": "",
                "username": "",
                "password": "",
                "disabled": false,
                "ignore_http_cache": false,
                "fetch_via_proxy": false,
                "category": {
                    "id": 22,
                    "user_id": 123,
                    "title": "Another category"
                },
                "icon": {
                    "feed_id": 42,
                    "icon_id": 84
                }
            }
        }
    ]
}
```

<h3 id="endpoint-update-entries">Update Entries <a class="anchor" href="#endpoint-update-entries" title="Permalink">¶</a></h3>

Request:

    PUT /v1/entries
    Content-Type: application/json

    {
        "entry_ids": [1234, 4567],
        "status": "read"
    }

Returns a `204` status code for success.

<h3 id="endpoint-toggle-bookmark">Toggle Entry Bookmark <a class="anchor" href="#endpoint-toggle-bookmark" title="Permalink">¶</a></h3>

Request:

    PUT /v1/entries/1234/bookmark

Returns a `204` status code for success.

<h3 id="endpoint-get-enclosure">Get Enclosure <a class="anchor" href="#endpoint-get-enclosure" title="Permalink">¶</a></h3>

Request:

    GET /v1/enclosures/{enclosureID}

Response:

```json
{
  "id": 278,
  "user_id": 1,
  "entry_id": 195,
  "url": "https://example.org/file",
  "mime_type": "application/octet-stream",
  "size": 0,
  "media_progression": 0
}
```

<div class="info">
This API endpoint has been available since Miniflux v2.2.0.
</div>

<h3 id="endpoint-update-enclosure">Update Enclosure <a class="anchor" href="#endpoint-update-enclosure" title="Permalink">¶</a></h3>

Request:

    PUT /v1/enclosures/{enclosureID}

    {
        "media_progression": 42
    }

Returns a `204` status code for success.

<div class="info">
This API endpoint has been available since Miniflux v2.2.0.
</div>

<h3 id="endpoint-get-categories">Get Categories <a class="anchor" href="#endpoint-get-categories" title="Permalink">¶</a></h3>

Request:

    GET /v1/categories

Response:

```json
[
    {"title": "All", "user_id": 267, "id": 792, "hide_globally": false},
    {"title": "Engineering Blogs", "user_id": 267, "id": 793, "hide_globally": false}
]
```

Since Miniflux 2.0.46, you can pass the argument `?counts=true` to include `total_unread` and `feed_count` in the response:

Request:

    GET /v1/categories?counts=true

Response:

```json
[
  {
    "id": 1,
    "title": "All",
    "user_id": 1,
    "hide_globally": false,
    "feed_count": 7,
    "total_unread": 268
  }
]
```

<h3 id="endpoint-create-category">Create Category <a class="anchor" href="#endpoint-create-category" title="Permalink">¶</a></h3>

Request:

    POST /v1/categories
    Content-Type: application/json

    {
        "title": "My category"
    }

Response:

``` json
{
    "id": 802,
    "user_id": 267,
    "title": "My category",
    "hide_globally": false
}
```

Since Miniflux 2.2.8, you can also pass the `hide_globally` field when creating a category:

    POST /v1/categories
    Content-Type: application/json

    {
        "title": "My category",
        "hide_globally": true
    }

<h3 id="endpoint-update-category">Update Category <a class="anchor" href="#endpoint-update-category" title="Permalink">¶</a></h3>

Request:

    PUT /v1/categories/802
    Content-Type: application/json

    {
        "title": "My new title"
    }

Response:

``` json
{
    "id": 802,
    "user_id": 267,
    "title": "My new title",
    "hide_globally": false
}
```

Since Miniflux 2.2.8, you can also pass the `hide_globally` field when updating a category:

    PUT /v1/categories/123
    Content-Type: application/json

    {
        "hide_globally": true
    }

<h3 id="endpoint-refresh-category">Refresh Category Feeds<a class="anchor" href="#endpoint-refresh-category" title="Permalink">¶</a></h3>

Request:

    PUT /v1/categories/123/refresh

<ul>
    <li>Returns <code>204</code> status code for success.</li>
    <li>Category feeds are refreshed in a background process.</li>
</ul>

<div class="info">
This API endpoint is available since Miniflux v2.0.42.
</div>

<h3 id="endpoint-delete-category">Delete Category <a class="anchor" href="#endpoint-delete-category" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/categories/802

Returns a `204` status code when successful.

<h3 id="endpoint-mark-category-entries-as-read">Mark Category Entries as Read <a class="anchor" href="#endpoint-mark-category-entries-as-read" title="Permalink">¶</a></h3>

Request:

    PUT /v1/categories/123/mark-all-as-read

Returns `204 Not Content` status code for success.

<div class="info">
This API endpoint is available since Miniflux v2.0.26.
</div>

<h3 id="endpoint-export">OPML Export <a class="anchor" href="#endpoint-export" title="Permalink">¶</a></h3>

Request:

    GET /v1/export

The response is a XML document (OPML file).

<div class="info">
This API call is available since Miniflux v2.0.1.
</div>

<h3 id="endpoint-import">OPML Import <a class="anchor" href="#endpoint-import" title="Permalink">¶</a></h3>

Request:

    POST /v1/import

    XML data

- The body is your OPML file (XML).
- Returns `201 Created` if imported successfully.

Response:

``` json
{
  "message": "Feeds imported successfully"
}
```

<div class="info">
This API call is available since Miniflux v2.0.7.
</div>

<h3 id="endpoint-create-user">Create User <a class="anchor" href="#endpoint-create-user" title="Permalink">¶</a></h3>

Request:

    POST /v1/users
    Content-Type: application/json

    {
        "username": "bob",
        "password": "test123",
        "is_admin": false
    }

Available Fields:

| Field                      | Type        |
| -------------------------- | ----------- |
| `username`                 | `string`    |
| `password`                 | `string`    |
| `google_id`                | `string`    |
| `openid_connect_id`        | `string`    |
| `is_admin`                 | `boolean`   |

Response:

``` json
{
    "id": 270,
    "username": "bob",
    "theme": "system_serif",
    "language": "en_US",
    "timezone": "UTC",
    "entry_sorting_direction": "desc",
    "stylesheet": "",
    "google_id": "",
    "openid_connect_id": "",
    "entries_per_page": 100,
    "keyboard_shortcuts": true,
    "show_reading_time": true,
    "entry_swipe": true,
    "last_login_at": null
}
```

<div class="info">
You must be an administrator to create users.
</div>

<h3 id="endpoint-update-user">Update User <a class="anchor" href="#endpoint-update-user" title="Permalink">¶</a></h3>

Request:

    PUT /v1/users/270
    Content-Type: application/json

    {
        "username": "joe"
    }

Available fields:

| Field                      | Type        | Example         |
| -------------------------- | ----------- |-----------------|
| `username`                 | `string`    |                 |
| `password`                 | `string`    |                 |
| `theme`                    | `string`    | "dark_serif"    |
| `language`                 | `string`    | "fr_FR"         |
| `timezone`                 | `string`    | "Europe/Paris"  |
| `entry_sorting_direction`  | `string`    | "desc" or "asc" |
| `stylesheet`               | `string`    |                 |
| `google_id`                | `string`    |                 |
| `openid_connect_id`        | `string`    |                 |
| `entries_per_page`         | `int`       |                 |
| `is_admin`                 | `boolean`   |                 |
| `keyboard_shortcuts`       | `boolean`   |                 |
| `show_reading_time`        | `boolean`   |                 |
| `entry_swipe`              | `boolean`   |                 |

Response:

``` json
{
    "id": 270,
    "username": "joe",
    "theme": "system_serif",
    "language": "en_US",
    "timezone": "America/Los_Angeles",
    "entry_sorting_direction": "desc",
    "stylesheet": "",
    "google_id": "",
    "openid_connect_id": "",
    "entries_per_page": 100,
    "keyboard_shortcuts": true,
    "show_reading_time": true,
    "entry_swipe": true,
    "last_login_at": "2021-01-05T06:46:06.461189Z"
}
```

<div class="info">
You must be an administrator to update users.
</div>

<h3 id="endpoint-me">Get Current User <a class="anchor" href="#endpoint-me" title="Permalink">¶</a></h3>

Request:

    GET /v1/me

Response:

``` json
{
    "id": 1,
    "username": "admin",
    "is_admin": true,
    "theme": "dark_serif",
    "language": "en_US",
    "timezone": "America/Vancouver",
    "entry_sorting_direction": "desc",
    "stylesheet": "",
    "google_id": "",
    "openid_connect_id": "",
    "entries_per_page": 100,
    "keyboard_shortcuts": true,
    "show_reading_time": true,
    "entry_swipe": true,
    "last_login_at": "2021-01-05T04:51:45.118524Z"
}
```

<div class="info">
This API endpoint is available since Miniflux v2.0.8.
</div>

<h3 id="endpoint-get-user">Get User <a class="anchor" href="#endpoint-get-user" title="Permalink">¶</a></h3>

Request:

    # Get user by user ID
    GET /v1/users/270

    # Get user by username
    GET /v1/users/foobar

Response:

``` json
{
    "id": 270,
    "username": "test",
    "is_admin": false,
    "theme": "light_serif",
    "language": "en_US",
    "timezone": "America/Los_Angeles",
    "entry_sorting_direction": "desc",
    "stylesheet": "",
    "google_id": "",
    "openid_connect_id": "",
    "entries_per_page": 100,
    "keyboard_shortcuts": true,
    "show_reading_time": true,
    "entry_swipe": true,
    "last_login_at": "2021-01-04T20:57:34.447789-08:00"
}
```

<div class="info">
You must be an administrator to fetch users.
</div>

<h3 id="endpoint-get-users">Get Users <a class="anchor" href="#endpoint-get-users" title="Permalink">¶</a></h3>

Request:

    GET /v1/users

Response:

``` json
[
    {
        "id": 270,
        "username": "test",
        "is_admin": false,
        "theme": "light_serif",
        "language": "en_US",
        "timezone": "America/Los_Angeles",
        "entry_sorting_direction": "desc",
        "stylesheet": "",
        "google_id": "",
        "openid_connect_id": "",
        "entries_per_page": 100,
        "keyboard_shortcuts": true,
        "show_reading_time": true,
        "entry_swipe": true,
        "last_login_at": "2021-01-04T20:57:34.447789-08:00"
    }
]
```

<div class="info">
You must be an administrator to fetch users.
</div>

<h3 id="endpoint-delete-user">Delete User <a class="anchor" href="#endpoint-delete-user" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/users/270

<div class="info">
You must be an administrator to delete users.
</div>

<h3 id="integrations-status">Integrations Status <a class="anchor" href="#integrations-status" title="Permalink">¶</a></h3>

This API endpoint returns `true` if the authenticated user has enabled an integration to save entries to a third-party service (e.g., Wallabag, Shiori, Shaarli). Note that the Google Reader API and Fever API are not considered.

Request:

    GET /integrations/status

Response:

    {"has_integrations": true}

<div class="info">
This API endpoint is available since Miniflux v2.2.2.
</div>

<h3 id="endpoint-mark-user-entries-as-read">Mark User Entries as Read <a class="anchor" href="#endpoint-mark-user-entries-as-read" title="Permalink">¶</a></h3>

Request:

    PUT /v1/users/123/mark-all-as-read

Returns `204 Not Content` status code for success.

<div class="info">
This API endpoint is available since Miniflux v2.0.26.
</div>

<h3 id="endpoint-counters">Fetch Read/Unread Counters <a class="anchor" href="#endpoint-counters" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/counters

Response Example:

``` json
{
  "reads": {
    "1": 12,
    "3": 1,
    "4": 1
  },
  "unreads": {
    "1": 7,
    "3": 99,
    "4": 14
  }
}
```

<div class="info">
This endpoint is available since Miniflux 2.0.37.
</div>

<h3 id="endpoint-get-api-keys">Get API Keys <a class="anchor" href="#endpoint-get-api-keys" title="Permalink">¶</a></h3>

Request:

    GET /v1/api-keys

Response:

```json
[
    {
        "id": 1,
        "user_id": 1,
        "description": "My API Key",
        "token": "1234567890abcdef1234567890abcdef",
        "created_at": "2023-10-07T03:52:50.013556Z",
        "last_used_at": "2023-10-07T03:52:50.013556Z"
    }
]
```

<div class="info">
This endpoint is available since Miniflux 2.2.9.
</div>

<h3 id="endpoint-create-api-key">Create API Key <a class="anchor" href="#endpoint-create-api-key" title="Permalink">¶</a></h3>

Request:

    POST /v1/api-keys

    {"description": "My API Key"}

Response:

```json
{
    "id": 1,
    "user_id": 1,
    "description": "My API Key",
    "token": "1234567890abcdef1234567890abcdef",
    "created_at": "2023-10-07T03:52:50.013556Z",
    "last_used_at": "2023-10-07T03:52:50.013556Z"
}
```

<div class="info">
This endpoint is available since Miniflux 2.2.9.
</div>

<h3 id="endpoint-delete-api-key">Delete API Key <a class="anchor" href="#endpoint-delete-api-key" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/api-keys/1

Returns a `204 No Content` status code for success.

<div class="info">
This endpoint is available since Miniflux 2.2.9.
</div>

<h3 id="endpoint-healthcheck">Healthcheck <a class="anchor" href="#endpoint-healthcheck" title="Permalink">¶</a></h3>

The healthcheck endpoint is useful for monitoring and load-balancer configuration.

Request:

    GET /healthcheck

Response:

```
OK
```

- Returns a `200 OK` status code when the service is running and the database connectivity is healthy.
- This route takes into consideration the base path.

<h3 id="endpoint-liveness">Liveness<a class="anchor" href="#endpoint-liveness" title="Permalink">¶</a></h3>

Request:

    GET /liveness

Alternatively:

    GET /healthz

Response:

```
OK
```

- Returns a `200 OK` status code when the service is running.
- This endpoint does not check database connectivity.
- These routes do not take the base path into consideration and are available at the root of the Miniflux instance.
- This endpoint can be used as Kubernetes liveness probe.
- Available since Miniflux 2.2.9.

<h3 id="endpoint-readiness">Readiness<a class="anchor" href="#endpoint-readiness" title="Permalink">¶</a></h3>

Request:

    GET /readiness

Alternatively:

    GET /readyz

Response:

```
OK
```

- Returns a `200 OK` status code when the service is ready to accept requests.
- This endpoint checks database connectivity.
- These routes do not take the base path into consideration and are available at the root of the Miniflux instance.
- This endpoint can be used as Kubernetes readiness probe.
- Available since Miniflux 2.2.9.

<h3 id="deprecated-endpoint-version">Application version <a class="anchor" href="#deprecated-endpoint-version" title="Permalink">¶</a></h3>

The version endpoint returns Miniflux build version.

Request:

    GET /version

Response:

```
2.0.22
```

<div class="info">
This API endpoint is available since Miniflux v2.0.22 and it's deprecated since version 2.0.49.
</div>

<h3 id="endpoint-version">Application version and build information <a class="anchor" href="#endpoint-version" title="Permalink">¶</a></h3>

The version endpoint returns Miniflux version and build information.

Request:

    GET /v1/version

Response:

```json
{
    "version":"2.0.49",
    "commit":"69779e795",
    "build_date":"2023-10-14T20:12:04-0700",
    "go_version":"go1.21.1",
    "compiler":"gc",
    "arch":"amd64",
    "os":"linux"
}
```

<div class="info">
This API endpoint is available since Miniflux v2.0.49.
</div>
