---
title: Integration with External Services
url: /docs/services.html
---

External services available:

- [Fever API](#fever)
- [Google Reader API](#google-reader)
- [Pinboard](#pinboard)
- [Instapaper](#instapaper)
- [Pocket](#pocket)
- [Wallabag](#wallabag)
- [Notion](#notion)
- [Nunux Keeper](#nunux-keeper)
- [Telegram Bot](#telegram-bot)

<h2 id="fever">Fever API <a class="anchor" href="#fever" title="Permalink">¶</a></h2>

Miniflux implements the [Fever API](https://feedafever.com/api) in addition to its own [REST API](api.html). The
Fever API allows you to use existing mobile applications to read your
feeds instead of the web user interface.

To activate the Fever API, go to the integration section and choose a username/password.

![Fever API](/images/fever.png)

### Compatible Apps

- [Reeder](http://reederapp.com/) (iOS/macOS)
- [Unread](https://www.goldenhillsoftware.com/unread/) (iOS)
- [FeedMe](https://play.google.com/store/apps/details?id=com.seazon.feedme&hl=en) (Android)
- [NewsFlash](https://gitlab.com/news-flash/news_flash_gtk) (Linux)
- [Fluent Reader](https://hyliu.me/fluent-reader/) (Windows and Mac)

### Notes

- Saving an entry will add a new bookmark and save the article
- Only the JSON format is supported
- Refreshing feeds is not possible with Reeder because no user information is sent
- Links, sparks, and kindlings are not supported

<h2 id="google-reader">Google Reader API <a class="anchor" href="#google-reader" title="Permalink">¶</a></h2>

Miniflux implements the Google Reader API.

To activate the Google Reader API, go to the integration section and choose a username/password.

![Google Reader API](/images/google_reader.png)

### Compatible Apps

- [Reeder >= 5](http://reederapp.com/) (iOS/macOS)

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
- If `Send only URL` is checked, Miniflux sends only the URL of the item being saved, instead of the item's page body content.

<h2 id="notion">Notion <a class="anchor" href="#notion" title="Permalink">¶</a></h2>

[Notion](https://notion.so/) is a freemium productivity and note-taking web application

- To create a new API integration, go to `https://www.notion.so/my-integrations` then click on `New Integration`
- Fill in all the need field and select your board of choice
- Once `Submit` you will be redirect to `Secret` tab in your integration page. generate a secret which look like this `secret_XXXXXXX`
- Create a new page and then head to `...` on the top right corner
- Scroll down to `Add Connections` and select API integration created before
- Again click on `...` on the same page and then click on `Copy link` you should have something like this: `https://www.notion.so/<user>/<page-name>-1429989fe8ac4effbc8f57f56486db54?pvs=4` your page id will be this one`1429989fe8ac4effbc8f57f56486db54`

<h2 id="nunux-keeper">Nunux Keeper <a class="anchor" href="#nunux-keeper" title="Permalink">¶</a></h2>

[Nunux Keeper](https://keeper.nunux.org/) is a *"personal content
curation service"*. It's an alternative to Pocket or Wallabag.

![Nunux Reader](/images/nunux_reader.png)

- The API URL is the root URL of your instance, for example, if you are using the hosted version: `https://api.nunux.org/keeper`.
- To create a new API key, go to the settings tab: "API key" and choose "Regenerate API key".

![Nunux Reader API Key](/images/nunux_reader_api_key.png)

<h2 id="telegram-bot">Telegram Bot <a class="anchor" href="#telegram-bot" title="Permalink">¶</a></h2>

![Telegram Bot Integration](/images/telegram-bot-form.png)

[Telegram](https://telegram.org/) is a free, cross-platform, cloud-based instant messaging software.
You can enable integration via Telegram Bot API and new entries will be pushed to the specific chat when fetched.

You should [Create a bot using @BotFather](https://core.telegram.org/bots#6-botfather) and get the bot token from it.

![Create a bot using @BotFater](/images/telegram-bot-get-bot-token-from-bot-father.png)

Then invite [@getidsbot](https://t.me/getidsbot) to your channel or group,
or just send `/about` to the bot if you want to push the message to the direct message chat to you.
Get the chat id field from the message. You can stop or remove the bot after getting the id.

![Get chat id using @getidsbot](/images/telegram-bot-get-chat-id-from-bot.png)

Now invite your bot to your channel or group, or send some message to it if you want to push the message to the direct message.

Fill in the values and enable the telegram bot integration in miniflux, and new entries should be pushed to the chat you have selected now.
