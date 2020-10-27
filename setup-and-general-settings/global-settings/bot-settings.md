---
description: Information about global bot settings.
---

# Bot settings

The bot settings menu lets you change global settings that affect all trading pairs.

To change them, go to **Global settings** &gt; **Bot**

![](../../.gitbook/assets/image%20%2841%29.png)

## Settings descriptions

Below you'll find detailed descriptions for bot settings.

### Watch mode

{% tabs %}
{% tab title="Description" %}
When set to true, Gunbot will process the configured pairs, but will not place actual buy or sell orders. Good for testing.

This option is meant for blocking strategy buy and sell orders for spot trading, it may not apply to every single order type.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `WATCH_MODE`
{% endtab %}
{% endtabs %}

### Multiple base

{% tabs %}
{% tab title="Description" %}
Use this option to trade pairs with cross-over between quote and base \(for example BTC-ETH and ETH-ADA\).

When enabled, Gunbot won't sell all available quote units when selling, instead it will only sell the invested funds \(as defined in the trading limit\). Also affects the TradingView add-on.

Only enable this when you really need it. If you experience errors when selling, try turning off this settings
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MULTIPLE_BASE`
{% endtab %}
{% endtabs %}

### Clean cache

{% tabs %}
{% tab title="Description" %}
This parameter forces the Gunbot cache to be cleaned by restarting the bot every x hours.

Only set this to a low value when your bot actually has problems not trading after a longer period of use. If you for example see heap memory related warnings in your logs, it might be a good idea to clean cache more often.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents time in hours.

**Default value:** 2
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BOT_CCLEAN`
{% endtab %}
{% endtabs %}

