title: API Reference
template: doc
uri: docs/api.html
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
    - [Get Feeds](#endpoint-get-feeds)
    - [Get Feed](#endpoint-get-feed)
    - [Get Feed Icon](#endpoint-get-feed-icon)
    - [Create Feed](#endpoint-create-feed)
    - [Update Feed](#endpoint-update-feed)
    - [Refresh Feed](#endpoint-refresh-feed)
    - [Refresh all Feeds](#endpoint-refresh-all-feeds)
    - [Remove Feed](#endpoint-remove-feed)
    - [Get Feed Entry](#endpoint-get-feed-entry)
    - [Get Entry](#endpoint-get-entry)
    - [Get Feed Entries](#endpoint-get-feed-entries)
    - [Get Entries](#endpoint-get-entries)
    - [Update Entries](#endpoint-update-entries)
    - [Toggle Entry Bookmark](#endpoint-toggle-bookmark)
    - [Get Categories](#endpoint-get-categories)
    - [Create Category](#endpoint-create-category)
    - [Update Category](#endpoint-update-category)
    - [Delete Category](#endpoint-delete-category)
    - [OPML Export](#endpoint-export)
    - [OPML Import](#endpoint-import)
    - [Create User](#endpoint-create-user)
    - [Update User](#endpoint-update-user)
    - [Get Current User](#endpoint-me)
    - [Get User](#endpoint-get-user)
    - [Get Users](#endpoint-get-users)
    - [Delete User](#endpoint-delete-user)
    - [Healthcheck](#endpoint-healthcheck)
    - [Version](#endpoint-version)

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

- Repository: <https://github.com/miniflux/v2/tree/master/client>
- Reference: <https://godoc.org/miniflux.app/client>

Installation:

```bash
go get -u miniflux.app/client
```

Usage Example:

```go
package main

import (
    "fmt"

    miniflux "miniflux.app/client"
)

func main() {
    // Authentication using username/password.
    client := miniflux.New("https://miniflux.example.org", "admin", "secret")

    // Authentication using API token.
    client := miniflux.New("https://miniflux.example.org", "My secret token")

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

``` json
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

<h3 id="endpoint-get-feeds">Get Feeds <a class="anchor" href="#endpoint-get-feeds" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds

Response:

``` json
[
    {
        "id": 42,
        "user_id": 123,
        "title": "Example Feed",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "rewrite_rules": "",
        "scraper_rules": "",
        "crawler": false,
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_count": 0,
        "parsing_error_message": "",
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

<h3 id="endpoint-get-feed">Get Feed <a class="anchor" href="#endpoint-get-feed" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/42

Response:

``` json
{
    "id": 42,
    "user_id": 123,
    "title": "Example Feed",
    "site_url": "http://example.org",
    "feed_url": "http://example.org/feed.atom",
    "rewrite_rules": "",
    "scraper_rules": "",
    "crawler": false,
    "checked_at": "2017-12-22T21:06:03.133839-05:00",
    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
    "parsing_error_count": 0,
    "parsing_error_message": "",
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

<h3 id="endpoint-get-feed-icon">Get Feed Icon <a class="anchor" href="#endpoint-get-feed-icon" title="Permalink">¶</a></h3>

Request:

    GET /v1/feeds/42/icon

Response:

``` json
{
    "id": 262,
    "data": "image/png;base64,iVBORw0KGgoAAA....",
    "mime_type": "image/png"
}
```

If the feed doesn't have any favicon, a 404 is returned.

<h3 id="endpoint-create-feed">Create Feed <a class="anchor" href="#endpoint-create-feed" title="Permalink">¶</a></h3>

Request:

    POST /v1/feeds
    Content-Type: application/json

    {
        "feed_url": "http://example.org/feed.atom",
        "category_id": 22
    }

Response:

``` json
{
    "feed_id": 262,
}
```

Required fields:

  - `feed_url`: Feed URL (string)
  - `category_id`: Category ID (int)

Optional fields:

- `username`: Feed username (string)
- `password`: Feed password (string)
- `crawler`: Enable/Disable scraper (boolean)
- `user_agent`: Custom user agent for the feed (string)
- `scraper_rules`: List of scraper rules (string) - Available since Miniflux 2.0.19
- `rewrite_rules`: List of rewrite rules (string) - Available since Miniflux 2.0.19

<h3 id="endpoint-update-feed">Update Feed <a class="anchor" href="#endpoint-update-feed" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/42
    Content-Type: application/json

    {
        "title": "New Feed Title",
        "category_id": 22
    }

Response:

``` json
{
    "id": 42,
    "user_id": 123,
    "title": "New Feed Title",
    "site_url": "http://example.org",
    "feed_url": "http://example.org/feed.atom",
    "rewrite_rules": "",
    "scraper_rules": "",
    "crawler": false,
    "checked_at": "2017-12-22T21:06:03.133839-05:00",
    "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
    "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
    "parsing_error_count": 0,
    "parsing_error_message": "",
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

  - `feed_url`: (string)
  - `site_url`: (string)
  - `title`: (string)
  - `category_id`: (int)
  - `scraper_rules`: (string)
  - `rewrite_rules`: (string)
  - `crawler`: (boolean)
  - `username`: (string)
  - `password`: (string)
  - `user_agent`: Custom user agent for the feed (string)

<h3 id="endpoint-refresh-feed">Refresh Feed <a class="anchor" href="#endpoint-refresh-feed" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/42/refresh

<div class="info">
<ul>
    <li>Returns <code>204</code> status code for success.</li>
    <li>This API call is synchronous and can takes hundred of milliseconds.</li>
</ul>
</div>

<h3 id="endpoint-refresh-all-feeds">Refresh all Feeds <a class="anchor" href="#endpoint-refresh-all-feeds" title="Permalink">¶</a></h3>

Request:

    PUT /v1/feeds/refresh

<div class="info">
<ul>
    <li>Returns <code>204</code> status code for success.</li>
    <li>Feeds are refreshed in a background process.</li>
    <li>Available since Miniflux 2.0.21</li>
</ul>
</div>

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
    "status": "read",
    "starred": false,
    "feed": {
        "id": 42,
        "user_id": 123,
        "title": "New Feed Title",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "rewrite_rules": "",
        "scraper_rules": "",
        "crawler": false,
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_count": 0,
        "parsing_error_message": "",
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

<div class="info">
The field <code>comments_url</code> is available since Miniflux v2.0.5.
</div>

<h3 id="endpoint-get-entry">Get Entry <a class="anchor" href="#endpoint-get-entry" title="Permalink">¶</a></h3>

Request:

    GET /v1/entries/888

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
    "status": "read",
    "starred": false,
    "feed": {
        "id": 42,
        "user_id": 123,
        "title": "New Feed Title",
        "site_url": "http://example.org",
        "feed_url": "http://example.org/feed.atom",
        "rewrite_rules": "",
        "scraper_rules": "",
        "crawler": false,
        "checked_at": "2017-12-22T21:06:03.133839-05:00",
        "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
        "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
        "parsing_error_count": 0,
        "parsing_error_message": "",
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
- `before_entry_id` (int64, available since Miniflux 2.0.9)
- `after_entry_id` (int64, available since Miniflux 2.0.9)
- `starred` (boolean, available since Miniflux 2.0.9)
- `search`: search query (text, available since Miniflux 2.0.10)
- `category_id`: filter by category (int, available since Miniflux 2.0.19)

Response:

``` json
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
            "status": "read",
            "starred": false,
            "feed": {
                "id": 42,
                "user_id": 123,
                "title": "New Feed Title",
                "site_url": "http://example.org",
                "feed_url": "http://example.org/feed.atom",
                "rewrite_rules": "",
                "scraper_rules": "",
                "crawler": false,
                "checked_at": "2017-12-22T21:06:03.133839-05:00",
                "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                "parsing_error_count": 0,
                "parsing_error_message": "",
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
```

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
  - `before_entry_id` (int64, available since Miniflux 2.0.9)
  - `after_entry_id` (int64, available since Miniflux 2.0.9)
  - `starred` (boolean, available since Miniflux 2.0.9)
  - `search`: search query (text, available since Miniflux 2.0.10)
  - `category_id`: filter by category (int, available since Miniflux 2.0.24)

Response:

``` json
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
            "status": "unread",
            "starred": false,
            "feed": {
                "id": 42,
                "user_id": 123,
                "title": "New Feed Title",
                "site_url": "http://example.org",
                "feed_url": "http://example.org/feed.atom",
                "rewrite_rules": "",
                "scraper_rules": "",
                "crawler": false,
                "checked_at": "2017-12-22T21:06:03.133839-05:00",
                "etag_header": "KyLxEflwnTGF5ecaiqZ2G0TxBCc",
                "last_modified_header": "Sat, 23 Dec 2017 01:04:21 GMT",
                "parsing_error_count": 0,
                "parsing_error_message": "",
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
```

<h3 id="endpoint-update-entries">Update Entries <a class="anchor" href="#endpoint-update-entries" title="Permalink">¶</a></h3>

Request:

    PUT /v1/entries
    Content-Type: application/json

    {
        "entry_ids": [1234, 4567],
        "status": "read"
    }

Returns `204` status code for success.

<h3 id="endpoint-toggle-bookmark">Toggle Entry Bookmark <a class="anchor" href="#endpoint-toggle-bookmark" title="Permalink">¶</a></h3>

Request:

    PUT /v1/entries/1234/bookmark

Returns `204` status code for success.

<h3 id="endpoint-get-categories">Get Categories <a class="anchor" href="#endpoint-get-categories" title="Permalink">¶</a></h3>

Request:

    GET /v1/categories

Response:

``` json
[
    {"title": "All", "user_id": 267, "id": 792},
    {"title": "Engineering Blogs", "user_id": 267, "id": 793}
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
    "title": "My category"
}
```

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
    "title": "My new title"
}
```

<h3 id="endpoint-delete-category">Delete Category <a class="anchor" href="#endpoint-delete-category" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/categories/802

Returns a `204` status code when successful.

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

Response:

``` json
{
    "id": 270,
    "username": "bob",
    "language": "en_US",
    "timezone": "UTC",
    "theme": "default",
    "entry_sorting_direction": "asc"
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

  - `username`: (string)
  - `password`: (string)
  - `is_admin`: (boolean)
  - `theme`: (string)
  - `language`: (string)
  - `timezone`: (string)
  - `entry_sorting_direction`: "desc" or "asc" (available since Miniflux
    2.0.9)

Response:

``` json
{
    "id": 270,
    "username": "joe",
    "language": "en_US",
    "timezone": "UTC",
    "theme": "default",
    "entry_sorting_direction": "asc"
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
    "theme": "default",
    "language": "en_US",
    "timezone": "America/Vancouver",
    "entry_sorting_direction": "desc",
    "last_login_at": "2018-06-01T19:54:30.723051-07:00",
    "extra": {}
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
    "username": "bob",
    "is_admin": false,
    "language": "en_US",
    "timezone": "UTC",
    "theme": "default",
    "entry_sorting_direction": "asc",
    "last_login_at": "2017-12-27T16:40:58.841841-05:00",
    "extra": {
        "google_id": "42424242424242"
    }
}
```

<div class="info">
You must be an administrator to fetch users. The extra field is a dictionary of optional values.
</div>

<h3 id="endpoint-get-users">Get Users <a class="anchor" href="#endpoint-get-users" title="Permalink">¶</a></h3>

Request:

    GET /v1/users

Response:

``` json
[
    {
        "id": 270,
        "username": "bob",
        "is_admin": false,
        "language": "en_US",
        "timezone": "UTC",
        "theme": "default",
        "entry_sorting_direction": "asc",
        "last_login_at": "2017-12-27T16:40:58.841841-05:00",
        "extra": {}
    }
]
```

<div class="info">
You must be an administrator to fetch users.
The extra field is a dictionary of optional values.
</div>

<h3 id="endpoint-delete-user">Delete User <a class="anchor" href="#endpoint-delete-user" title="Permalink">¶</a></h3>

Request:

    DELETE /v1/users/270

<div class="info">
You must be an administrator to delete users.
</div>

<h3 id="endpoint-healthcheck">Healthcheck <a class="anchor" href="#endpoint-healthcheck" title="Permalink">¶</a></h3>

The healthcheck endpoint is useful for monitoring and load-balancer
configuration.

Request:

    GET /healthcheck

Response:

```
OK
```

Returns a status code 200 when the service is up.

<h3 id="endpoint-version">Version <a class="anchor" href="#endpoint-version" title="Permalink">¶</a></h3>

The version endpoint returns Miniflux build version.

Request:

    GET /version

Response:

```
2.0.22
```

<div class="info">
This API endpoint is available since Miniflux v2.0.22.
</div>
