---
description: >-
  Gunbot comes with a built-in Telegram bot that can be used to receive trade
  notifications, retrieve stats and control certain options of Gunbot right from
  from your Telegram client.
---

# Telegram notifications

{% hint style="info" %}
The Telegram bot \(formerly called CryptoSight\) is fully integrated into Gunbot. All you need to do to start the Telegram bot is enable Telegram notifications in your Gunbot settings.

After setting it up, type `/start` to your bot to open the menu.
{% endhint %}

## Demo video

{% embed url="https://www.youtube.com/watch?v=11fHq22nNFo" caption="" %}

## Get started

To set this up, go to **Settings** &gt; **Telegram**.

![](../../.gitbook/assets/image%20%2830%29.png)

## Steps to create a Telegram bot

Notifications work by first creating a personal bot on Telegram, Gunbot then connects to this bot to push notifications to you.

This is how to create a bot:

1. Talk to [@botfather](https://telegram.me/botfather). Create a new bot with the command /newbot and choose a name and username for your bot. Save the bot token shown.
2. Talk to [@myidbot](https://telegram.me/myidbot) to see your Chat ID, save it.
3. Enable Telegram notifications for Gunbot, and enter the token and ID you've just gathered. Use the ID for both the user and admin ID fields, this makes sure that only you can interact with the Telegram bot. _Alternatively, you can set a comma separated list for Admin ID, specifying multiple IDs who may interact with the bot._
4. Start a chat with the username you've picked for your bot, and hit the start button. If you don't see a start button, write "/start" and click on it afterwards.

{% hint style="warning" %}
Currently the Telegram functionality is not available on ARM platforms.
{% endhint %}

## Settings descriptions

Below you'll find detailed descriptions of all available parameters for Telegram notifications.

### Enabled

{% tabs %}
{% tab title="Description" %}
Enable this to have Gunbot send trade notifications through Telegram.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TELEGRAM_ENABLED`
{% endtab %}
{% endtabs %}

### Bot Nickname

{% tabs %}
{% tab title="Description" %}
Each trade notification starts with the nickname set here.

Use this to easily check from which bot instance the notifications have been sent.
{% endtab %}

{% tab title="Values" %}
**Values:** string

**Default value:** Gunbot
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TELEGRAM_NICK`
{% endtab %}
{% endtabs %}

### Token

{% tabs %}
{% tab title="Description" %}
The Telegram token for your bot.
{% endtab %}

{% tab title="Values" %}
**Values:** string

**Default value:** YOURTOKEN
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TOKEN`
{% endtab %}
{% endtabs %}

### Chat ID

{% tabs %}
{% tab title="Description" %}
The Chat ID for your bot to send its messages to.

**Valid options:**

_**"12345"**_

A positive integer, to send messages directly to a telegram user. Use this method when you just want to receive notifications for your personal use.

To find your telegram id, send /start to @MyTelegramID\_bot and it will respond with your ID.

_**"-12345"**_

A negative integer, to send messages to a group chat.

The easiest way to obtain a groups id, is to open [https://web.telegram.org](https://web.telegram.org) login, and navigate to the group. Now pay attention to the URL, you should see something like [https://web.telegram.org/\#/im?p=g12345](https://web.telegram.org/#/im?p=g12345) - the number after the p=g part is the group id.

This must be listed in chat\_id with a - symbol in front, in this case "-12345"
{% endtab %}

{% tab title="Values" %}
**Values:** string

**Default value:** 123456789
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `chat_id`
{% endtab %}
{% endtabs %}

### Admin ID / User ID

{% tabs %}
{% tab title="Description" %}
Defines which Telegram user IDs are allowed to interact with the bot. Must be used next to Chat ID.

When you leave this value empty, every Telegram user is able to interact with your Telegram bot.

Multiple IDs can be entered as a comma separated list.
{% endtab %}

{% tab title="Values" %}
**Values:** string

**Default value:** empty
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `user_id`
{% endtab %}
{% endtabs %}

### TG PL Only

{% tabs %}
{% tab title="Description" %}
When enabled notifications will be only sent for strategy sell orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TG_PL_ONLY`
{% endtab %}
{% endtabs %}

### Telegram Order Timeout

{% tabs %}
{% tab title="Description" %}
When set above 0, you'll receive a Telegram dialog to manually allow or deny every order Gunbot wants to place.

During the set amount of seconds you can choose to allow the order \(and it will immediately be placed\) or deny it. After the timeout passes without reply, the order will be placed at the exchange.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represent time in seconds.

**Default value:** 0
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TG_ORDER_TIMEOUT`
{% endtab %}
{% endtabs %}

### TG Test

{% tabs %}
{% tab title="Description" %}
Enable this to test if your Telegram bot is working.

A test message will be sent on the first round for each pair. Disable after verifying your bot works.

{% hint style="info" %}
This option is temporarily not available. It will be activated again in one of the next Gunbot updates.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TG_TEST`
{% endtab %}
{% endtabs %}

