---
title: Integration with External Services
url: /docs/services.html
---
<h2 id="fever">Fever API <a class="anchor" href="#fever" title="Permalink">¶</a></h2>

Miniflux implements the [Fever API](https://feedafever.com/api) in addition to its own [REST API](api.html). The
Fever API allows you to use existing mobile applications to read your
feeds instead of the web user interface.

To activate the Fever API, go into the integration section and choose a
username/password.

![Fever](/images/fever.png)

The API endpoint is `https://example.org/fever/`

### Compatible Apps

- [Reeder](http://reederapp.com/) (iOS/macOS)
- [Unread](https://www.goldenhillsoftware.com/unread/) (iOS)
- [Readably](https://play.google.com/store/apps/details?id=com.isaiasmatewos.readably) (Android)
- [FeedMe](https://play.google.com/store/apps/details?id=com.seazon.feedme&hl=en) (Android)

### Notes

- Saving an entry will add a new bookmark and save the article
- Only the JSON format is supported
- Refreshing feeds is not possible with Reeder because no user information is sent
- Links, sparks, and kindlings are not supported

<h2 id="pinboard">Pinboard <a class="anchor" href="#pinboard" title="Permalink">¶</a></h2>

You could save articles to [Pinboard](https://pinboard.in/).

![Pinboard](/images/pinboard.png)

To activate this service, go to the integration section and enter your
Pinboard API credentials. You must use the API token, not your password.

<h2 id="instapaper">Instapaper <a class="anchor" href="#instapaper" title="Permalink">¶</a></h2>

You could save articles to [Instapaper](https://www.instapaper.com/).

![Instapaper](/images/instapaper.png)

To activate this service, go to the integration section and enter your
Instapaper credentials.

<h2 id="pocket">Pocket <a class="anchor" href="#pocket" title="Permalink">¶</a></h2>

To configure the Pocket integration on your own Miniflux instance, you
need to create an application on Pocket's website.

Go to <https://getpocket.com/developer/apps/new> and create a new
application.

- You can define the *Pocket Consumer Key* for all users by using the environment variable `POCKET_CONSUMER_KEY`.
- Or, you can set the *Pocket Consumer Key* only for your user by using the form and hitting the update button.

![Pocket](/images/pocket_1.png)

If the environment variable is defined, the text field for the *Pocket
Consumer API key* will be hidden.

Make sure the environment variable `BASE_URL` is defined properly to
allow the authorization flow to work afterward, this variable should
point to your current instance address and/or the URL you have
set on your Pocket application.

Once the consumer key is configured, you need to get a *Pocket Access
Token*. This token could be fetched automatically by using the
authorization flow or manually by making some HTTP calls.

![Pocket Connection](/images/pocket_2.png)

The simplest solution is to use the link, **"Connect your Pocket
account"**. This method redirects the end user to Pocket's website and
ask for authorization.

![Pocket Authorization](/images/pocket_3.png)

After the authorization is given, Miniflux will fetch the *Pocket Access
Token* for you.

If you prefer to fetch *Pocket Access Token* manually, the process is
described in [Pocket's developer
documentation](https://getpocket.com/developer/docs/authentication).

<h2 id="wallabag">Wallabag <a class="anchor" href="#wallabag" title="Permalink">¶</a></h2>

[Wallabag](https://wallabag.org/) is a self-hosted application for
saving web pages.

![Wallabag](/images/wallabag.png)

- The API URL is the root URL of your instance, for example, if you have the hosted version use: `https://app.wallabag.it/`.
- To create a new API client, go to the section "API clients management" and choose "Create a new client".

<h2 id="nunux-keeper">Nunux Keeper <a class="anchor" href="#nunux-keeper" title="Permalink">¶</a></h2>

[Nunux Keeper](https://keeper.nunux.org/) is a *"personal content
curation service"*. It's an alternative to Pocket or Wallabag.

![Nunux Reader](/images/nunux_reader.png)

- The API URL is the root URL of your instance, for example, if you are using the hosted version: `https://api.nunux.org/keeper`.
- To create a new API key, go to the settings tab: "API key" and choose "Regenerate API key".

![Nunux Reader API Key](/images/nunux_reader_api_key.png)
