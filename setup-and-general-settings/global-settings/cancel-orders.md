---
description: Info about settings that control how Gunbot handles cancelling open orders.
---

# Cancel orders

Settings for cancelling orders are global, they affect all trading pairs.

To change them, go to **Global settings** &gt; **Cancel orders**

![](../../.gitbook/assets/image%20%2842%29.png)

## Settings descriptions

Below you'll find detailed descriptions for cancel orders settings.

### Cancel orders enabled

{% tabs %}
{% tab title="Description" %}
When set to true, the bot will cancel unfilled or partially filled orders when the price has moved away from the buy or sell price.

Set this to false if you also trade manually to prevent the bot from cancelling your open orders.

**Simulated Fill Or Kill \(FOK\)**

When an order is not or only partially filled and gets cancelled, Gunbot will attempt to fill the order by replacing it at current bid/ask.

For buy orders this means that FOK orders are sent as long as the number of quote units held are worth less than `TRADING_LIMIT` and the difference is higher than `MIN_VOLUME_TO_BUY`.

For sell orders this means that FOK orders are sent as long as the number of quote units held \(minus `KEEP_QUOTE`, if used\) are worth more than `MIN_VOLUME_TO_SELL` and bid is higher than the break-even point.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `CANCEL_ORDERS_ENABLED`
{% endtab %}
{% endtabs %}

### Cancel orders cycle cap

{% tabs %}
{% tab title="Description" %}
This only applies when using `MAKER_FEES` or `CANCEL_ONCAP`.

Set the number of rounds that pending orders need to be kept open. After this number of rounds passes, Gunbot will cancel the pending order.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a number of rounds.

**Default value:** 10
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `CANCEL_ORDERS_CYCLE_CAP`
{% endtab %}
{% endtabs %}

### Cancel orders oncap

{% tabs %}
{% tab title="Description" %}
Enabling this changes the behavior of cancelling orders: orders are cancelled after `CANCEL_ORDERS_CYCLE_CAP` passes.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `CANCEL_ONCAP`
{% endtab %}
{% endtabs %}

