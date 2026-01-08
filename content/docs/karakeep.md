---
title: Karakeep Integration
url: /docs/karakeep.html
---

[Karakeep](https://karakeep.app/) (formerly Hoarder) is a bookmark manager that can also be self-hosted.

![Karakeep](/images/karakeep.png)

- To create an API key, in Karakeep click on the user icon in the top right corner, then click on *User Settings* and go to *API Keys*. Here click on *New API Key*, give it a *Name* and click *Create*.
- In Miniflux, set the API endpoint to this template: `https://karakeep.example.com/api/v1/bookmarks`. This must be the full URL to the bookmarks endpoint otherwise saving will fail.
- Tags can be optionally specified as a comma-separated list.
