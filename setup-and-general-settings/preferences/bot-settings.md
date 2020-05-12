---
description: Information about global bot settings.
---

# Bot settings

The bot settings menu lets you change global settings that affect all trading pairs.

To change them, go to **Settings** &gt; **Bot Settings**.

![Global bot settings](https://gblobscdn.gitbook.com/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-M-pZS0pojIYvg7tzVzC%2F-M-pZWXjp8dGvucCl5xb%2Fassets_-l_rejuz9k0bdqxsqvuh_-lmxoqo1bz7zqcqvhbve_-lmxorov6_87omewq1_p_image.png?alt=media)

## Settings descriptions

Below you'll find detailed descriptions of all available parameters for bot settings. A few advanced settings are only available in the `config.js` file.

### Watch Mode

{% tabs %}
{% tab title="Description" %}
When set to true, Gunbot will process the configured pairs, but will not place actual buy or sell orders. Good for testing.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `WATCH_MODE`
{% endtab %}
{% endtabs %}

### Multiple Base

{% tabs %}
{% tab title="Description" %}
Use this option to trade pairs with cross-over between quote and base \(for example BTC-ETH and ETH-ADA\).

When enabled, Gunbot won't sell all available quote units when selling, instead it will only sell the invested funds \(as defined in the trading limit\). Also affects the TradingView add-on.

Only enable this when you really need it.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MULTIPLE_BASE`
{% endtab %}
{% endtabs %}

### Debug

{% tabs %}
{% tab title="Description" %}
Used to show debug messages in the bot, when set to true. Only use this if you really need to debug something.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `debug`
{% endtab %}
{% endtabs %}

### Verbose

{% tabs %}
{% tab title="Description" %}
Setting this to true will lead to more detailed information being shown in the console.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `VERBOSE`
{% endtab %}
{% endtabs %}

### Reserve Pile Up

{% tabs %}
{% tab title="Description" %}
When set to true, trading gains will be automatically added to the funds reserve and be kept out of further trading.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RESERVE_PILE_UP`
{% endtab %}
{% endtabs %}

### Bot Delay

{% tabs %}
{% tab title="Description" %}
Bot will delay processing a new pair for a set amount of seconds.

Useful for when Gunbot requests data faster than the exchange API is allowing you to do. As the needed delay depends on the amount of pairs and the speed your system needs to cycle pairs, there are no recommended values.

This is the global setting for bot delay, it is ignored when an exchange specific delay is set.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents time in seconds.

**Default value:** 1
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BOT_DELAY`
{% endtab %}
{% endtabs %}

### Bot CClean

{% tabs %}
{% tab title="Description" %}
This parameter forces the Gunbot cache to be cleaned by restarting the bot every x hours. This setting does not trigger `TRADES_TIMEOUT`.

Only set this to a low value when your bot actually has problems not trading after a longer period of use.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents time in hours.

**Default value:** 2
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BOT_CCLEAN`
{% endtab %}
{% endtabs %}

### Interval Ticker Update

{% tabs %}
{% tab title="Description" %}
Deprecated
{% endtab %}

{% tab title="Values" %}

{% endtab %}

{% tab title="Name" %}

{% endtab %}
{% endtabs %}

### Period Storage Ticker

{% tabs %}
{% tab title="Description" %}
Deprecated
{% endtab %}

{% tab title="Values" %}

{% endtab %}

{% tab title="Name" %}

{% endtab %}
{% endtabs %}

### Timeout Buy

{% tabs %}
{% tab title="Description" %}
This is an internal timeout that prevents the bot from buying again within the set amount of milliseconds after a buy order has been placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents time in milliseconds.

**Default value:** 59000
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `timeout_buy`
{% endtab %}
{% endtabs %}

### Timeout Sell

{% tabs %}
{% tab title="Description" %}
This is an internal timeout that prevents the bot from selling again within the set amount of milliseconds after a sell order has been placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents time in milliseconds.

**Default value:** 60000
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `timeout_sell`
{% endtab %}
{% endtabs %}

### Cancel Orders Enabled

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

### Cancel Orders Cycle Cap

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

### Cancel Orders Oncap

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

### Withdraw Threshold

{% tabs %}
{% tab title="Description" %}
Set the amount of BTC to be accumulated with `RESERVE_PILE_UP` before an automatic withdraw will be executed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents an amount of BTC

**Default value:** 0.5
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `withdraw_threshold`
{% endtab %}
{% endtabs %}

### Withdraw Address

{% tabs %}
{% tab title="Description" %}
Set a valid BTC wallet address to enable automatic withdraws each time the threshold is reached.

Please use this feature at your own risk only.
{% endtab %}

{% tab title="Values" %}
**Values:** string

**Default value:** YOURBTCADDRESSHERE
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `withdraw_address`
{% endtab %}
{% endtabs %}

### Json\_output

{% tabs %}
{% tab title="Description" %}
Sets the path for storing .json files. These files are where Gunbot saves its trading information.
{% endtab %}

{% tab title="Values" %}
**Values:** text, represents a folder location.

**Default value:** ./json
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `json_output`
{% endtab %}
{% endtabs %}

