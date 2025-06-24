---
title: Pocket Integration
url: /docs/pocket.html
---

**The Pocket integration has been removed from Miniflux 2.2.10 and later versions. [Pocket has been shut down on July 8, 2025.](https://support.mozilla.org/en-US/kb/future-of-pocket)**

To configure the Pocket integration on your own Miniflux instance, you need to create an application on Pocket's website.

Go to <https://getpocket.com/developer/apps/new> and create a new application.

- You can define the *Pocket Consumer Key* for all users by using the environment variable `POCKET_CONSUMER_KEY`.
- Alternatively, you can set the *Pocket Consumer Key* only for your user by using the form and clicking the update button.

![Pocket](/images/pocket_1.png)

If the environment variable is defined, the text field for the *Pocket Consumer API key* will be hidden.

Ensure the environment variable `BASE_URL` is defined properly to allow the authorization flow to work. This variable should point to your current instance address and/or the URL you have set on your Pocket application.

Once the consumer key is configured, you need to obtain a *Pocket Access Token*. This token can be fetched automatically using the authorization flow or manually by making some HTTP calls.

![Pocket Connection](/images/pocket_2.png)

The simplest solution is to use the link **"Connect your Pocket account"**. This method redirects the end user to Pocket's website and requests authorization.

![Pocket Authorization](/images/pocket_3.png)

After authorization is granted, Miniflux will fetch the *Pocket Access Token* for you.

If you prefer to fetch the *Pocket Access Token* manually, the process is described in [Pocket's developer documentation](https://getpocket.com/developer/docs/authentication).