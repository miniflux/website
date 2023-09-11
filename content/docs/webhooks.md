---
title: Webhooks
url: /docs/webhooks.html
---

[Webhooks](https://en.wikipedia.org/wiki/Webhook) are user-defined HTTP callbacks.

Miniflux can publish new feed entries to a custom API endpoint.
You can easily set up your own personal webhook receiver and extend Miniflux by writing your own scripts.

This feature is available since Miniflux 2.0.48.

Configuring a Webhook in Miniflux
-------------------------------

1. Go to **Settings > Integrations > Webhook**
2. Enter the URL of your webhook endpoint

The auto-generated secret is used to validate the payload.

Webhook Event Types
-------------------

Miniflux supports different types of event:

- `new_entries`: Event sent during a feed refresh when new entries are discovered
- `save_entry`: Event sent when the end-user save an entry

Webhook HTTP Request
--------------------

- HTTP Method: `POST`
- Content Type: `application/json`
- The HMAC signature is stored in the HTTP header: `X-Miniflux-Signature`
- The event type in stored in HTTP header `X-Miniflux-Event-Type` in addition to the request body
- The body contains a JSON payload

Validating Signatures from Miniflux
-----------------------------------

Miniflux will sign all inbound requests to your application with an `X-Miniflux-Signature` HTTP header.
The signature uses the HMAC-SHA256 hashing algorithm with the request body and the auto-generated secret key that is visible in **Settings > Integrations > Webhook**.

Example in PHP:

```php
// The secret value is visible in Settings > Integrations > Webhook
$secret = 'c4fdaf87900b72777cbe8e97d17b97a87d28ae078a462ef4ac5f2541fcf00ce6';

// Payload is the request body
$payload = file_get_contents('php://input');

$hmac = hash_hmac('sha256', $payload, $secret);

if (! hash_equals($hmac, $_SERVER['HTTP_X_MINIFLUX_SIGNATURE'])) {
    die('Incorrect signature!');
}
```

HTTP Authentication
-------------------

Miniflux supports HTTP Basic Authentication. 
This allows you to password protect the webhook URLs on your web server so that only you and Miniflux can access them.

You may provide a username and password via the following URL format: `https://username:password@webhook.example.org/`

New Entries Event Request
-------------------------

```
POST /your-webhook-endpoint HTTP/1.1

Content-Type: application/json
User-Agent: Miniflux/2.0.48
X-Miniflux-Signature: 7ff170cfd8c173fd5084e0f51ee6ac3eae8acff443f11f9168961cebf836e38f
X-Miniflux-Event-Type: new_entries

{
  "event_type": "new_entries",
  "feed": {
    "id": 8,
    "user_id": 1,
    "feed_url": "https://example.org/feed.xml",
    "site_url": "https://example.org",
    "title": "Example website",
    "checked_at": "2023-09-10T12:48:43.428196-07:00"
  },
  "entries": [
    {
      "id": 231,
      "user_id": 1,
      "feed_id": 3,
      "status": "unread",
      "hash": "1163a93ef12741b558a3b86d7e975c4c1de0152f3439915ed185eb460e5718d7",
      "title": "Example",
      "url": "https://example.org/article",
      "comments_url": "",
      "published_at": "2023-08-17T19:29:22Z",
      "created_at": "2023-09-10T12:48:43.428196-07:00",
      "changed_at": "2023-09-10T12:48:43.428196-07:00",
      "content": "<p>Some HTML content</p>",
      "share_code": "",
      "starred": false,
      "reading_time": 1,
      "enclosures": [{
          "id": 158,
          "user_id": 1,
          "entry_id": 231,
          "url": "https://example.org/podcast.mp3",
          "mime_type": "audio/mpeg",
          "size": 63451045,
          "media_progression": 0
      }],
      "tags": ["Some category", "Another label"]
    }
  ]
}
```

Save Entry Event Request
------------------------

```
POST /your-webhook-endpoint HTTP/1.1

Content-Type: application/json
User-Agent: Miniflux/2.0.48
X-Miniflux-Signature: 7ff170cfd8c173fd5084e0f51ee6ac3eae8acff443f11f9168961cebf836e38f
X-Miniflux-Event-Type: save_entry

{
  "event_type": "save_entry",
  "entry": {
    "id": 592,
    "user_id": 1,
    "feed_id": 9,
    "status": "read",
    "hash": "ed97d338e7ea23fd82d14f0623c1953a22ddda32ce406f0cdfbd14632db8ff8b",
    "title": "Some example",
    "url": "https://example.org/article",
    "comments_url": "",
    "published_at": "2023-09-10T19:13:40Z",
    "created_at": "2023-09-10T20:06:23.000332Z",
    "changed_at": "2023-09-11T00:39:49.615812Z",
    "content": "Some HTML content",
    "author": "",
    "share_code": "",
    "starred": false,
    "reading_time": 1,
    "enclosures": [
      {
        "id": 1492,
        "user_id": 1,
        "entry_id": 592,
        "url": "https://example.org/file.zip",
        "mime_type": "application/octet-stream",
        "size": 0,
        "media_progression": 0
      }
    ],
    "tags": [],
    "feed": {
      "id": 9,
      "user_id": 1,
      "feed_url": "https://example.org/feed.xml",
      "site_url": "https://example.org/,
      "title": "Example website",
      "checked_at": "2023-09-10T20:07:22.956279Z"
    }
  }
}
```

Notes
-----

- Miniflux does not retry webhook events on request failures.
