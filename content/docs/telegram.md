---
title: Telegram
url: /docs/telegram.html
---

[Telegram](https://telegram.org/) is free, cross-platform, cloud-based instant messaging software.

You can enable integration via Telegram Bot API and new entries will be pushed to the specific chat when fetched.

Telegram Bot Configuration
--------------------------

To create a bot, use [@BotFather](https://core.telegram.org/bots#6-botfather) and obtain the bot token from it.

![Create a bot using @BotFater](/images/telegram-bot-get-bot-token-from-bot-father.png)

Next, invite [@getidsbot](https://t.me/getidsbot) to your channel or group.
Alternatively, send `/about` to the bot if you want to push the message to your direct message chat.
Retrieve the chat ID from the message. You can stop or remove the bot after obtaining the ID.

![Get chat id using @getidsbot](/images/telegram-bot-get-chat-id-from-bot.png)

Now, invite your bot to your channel or group, or send a message to it if you want to push the message to your direct message chat.

Miniflux Configuration
----------------------

Fill in the values collected above, enable the Telegram bot integration in Miniflux, and new entries will be pushed to the chat you have selected.

![Telegram Bot Integration](/images/telegram-bot-form.png)

The topic ID is optional. You need to enable this feature explicitly in Telegram.

Telegram Messages
-----------------

Message with a comment button:

![Telegram notification with comments](/images/telegram-bot-notification-comments.png)

Message with a web page preview:

![Telegram notification with preview](/images/telegram-bot-notification-preview.png)
