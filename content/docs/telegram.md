---
title: Telegram
url: /docs/telegram.html
---

[Telegram](https://telegram.org/) is a free, cross-platform, cloud-based instant messaging software.
You can enable integration via Telegram Bot API and new entries will be pushed to the specific chat when fetched.

Telegram Bot Configuration
--------------------------

You should [create a bot using @BotFather](https://core.telegram.org/bots#6-botfather) and get the bot token from it.

![Create a bot using @BotFater](/images/telegram-bot-get-bot-token-from-bot-father.png)

Then invite [@getidsbot](https://t.me/getidsbot) to your channel or group,
or just send `/about` to the bot if you want to push the message to the direct message chat to you.
Get the chat id field from the message. You can stop or remove the bot after getting the id.

![Get chat id using @getidsbot](/images/telegram-bot-get-chat-id-from-bot.png)

Now invite your bot to your channel or group, or send some message to it if you want to push the message to the direct message.

Miniflux Configuration
----------------------

Fill in the values collected above and enable the Telegram bot integration in miniflux, and new entries should be pushed to the chat you have selected now.

![Telegram Bot Integration](/images/telegram-bot-form.png)

The topic ID is optional. You have to enable this feature explicitly in Telegram.

Telegram Messages
-----------------

Message with a comment button:

![Telegram notification with comments](/images/telegram-bot-notification-comments.png)

Message with a web page preview:

![Telegram notification with preview](/images/telegram-bot-notification-preview.png)
