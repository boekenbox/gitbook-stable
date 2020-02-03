---
description: Various extra strategy options.
---

# Misc settings

### Miscellaneous settings parameters

These strategy settings allow you to control your stop limit and other uncategorized features.

### Stop Limit

{% tabs %}
{% tab title="Description" %}
Sets a stop limit to sell a coin at a calculated loss.

After a stop limit sell order has been placed, the bot will go into buying mode after `TRADES_TIMEOUT` has passed and will buy again when market conditions meet your buying strategy.

Setting a stop limit at 60 would make sure that all holdings for a coin are sold when 60% value is lost, compared to the averaged bought price. E.g. average bought price is 100, stop limit is executed at 40 and all assets are sold.

{% hint style="info" %}
With margin trading, the stop limit is set as a ROE value. Setting it to 1 will lead to the stop limit triggering when ROE reaches -1. Use a value that includes your leverage.

It's recommended to use STOP\_BUY / STOP\_SELL instead, when possible.These are placed at the same time as the position is opened.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 60
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Stop limit | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOP_LIMIT`
{% endtab %}
{% endtabs %}

### SL Disable Buy

{% tabs %}
{% tab title="Description" %}
When set to true, buy orders will be disabled after a pair hits `STOP_LIMIT`.

For margin trading, buy orders will be disabled when a long position hits `STOP_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Stop limit | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SL_DISABLE_BUY`
{% endtab %}
{% endtabs %}

### SL Disable Sell

{% tabs %}
{% tab title="Description" %}
When set to true, sell orders will be disabled after a short position hits `STOP_LIMIT`.

{% hint style="success" %}
Specific to margin trading.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Stop limit | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SL_DISABLE_SELL`
{% endtab %}
{% endtabs %}

### Panic Sell

{% tabs %}
{% tab title="Description" %}
When set to true, all quote will be sold at market value as soon as possible. This may incur losses!

You should only enable this when you want to sell your current holdings immediately.

For margin trading, this setting will delete any open orders and close any position as soon as possible. Pairs are not automatically disabled afterwards.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Stop limit | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PANIC_SELL`
{% endtab %}
{% endtabs %}

### Trades Timeout

{% tabs %}
{% tab title="Description" %}
Sets a timeout between two trades for a single pair, in this time no trades will be placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents time in seconds.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Stop limit |
| RT buy |  |
| RT buyback |  |
| RT sell |  |
| Close |  |
| DCA buy |  |
| Strategy sell |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRADES_TIMEOUT`
{% endtab %}
{% endtabs %}

### Count Sell

{% tabs %}
{% tab title="Description" %}
Sets a maximum number of sell orders before automatically disabling a pair.

Setting this to 5 disables the pair after 5 strategy sell orders took place \(not including RT\). No further trades will happen until you re-enable the pair. The `COUNT_SELL` counter gets reset after a pair is re-enabled.

{% hint style="info" %}
This parameter is irrelevant for margin trading.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a number of strategy sell orders.

**Default value:** 9999
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `COUNT_SELL`
{% endtab %}
{% endtabs %}

### Maker Fees

{% tabs %}
{% tab title="Description" %}
**Bitmex:** When set to true, limit orders will placed as post only orders. If the order can be \(partially\) filled immediately, it will get cancelled by Bitmex. Using `PRE_ORDER` you can configure how far from bid/ask the order gets placed, you must use a negative value for `PRE_ORDER_GAP` for post only orders.

**Other exchanges:** When set to true, limit buy orders are placed at bid, limit sell orders are placed at ask. This increases the likelyhood that the trade is executed with maker fees.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Stop limit |
| RT buy |  |
| RT buyback |  |
| RT sell |  |
| Close |  |
| DCA buy |  |
| Strategy sell |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MAKER_FEES`
{% endtab %}
{% endtabs %}

### Ignore Trades Before

{% tabs %}
{% tab title="Description" %}
Optional parameter to force Gunbot to not consider any trades before the set timestamp.

Only ever use this when you know exactly what you are accomplishing, and for example want to prevent RT from being started on a pair where the last sell order resulted in a loss.

Use [https://currentmillis.com/](https://currentmillis.com/) to convert human readable time to unix timestamps, make sure to use the timestamp in milliseconds.
{% endtab %}

{% tab title="Values" %}
**Values:** unix timestamp in milliseconds

**Default value:** 0
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `IGNORE_TRADES_BEFORE`
{% endtab %}
{% endtabs %}

### Bought price

{% tabs %}
{% tab title="Description" %}
Exchanges often don't provide order information anymore on trades that happened longer ago. This parameter exists to manually specify a reference price per unit that Gunbot should consider when selling an asset for which no bought price is provided by the exchange.

This parameter should only be used as an override.

The override is only valid when no bought price can be retrieved from the exchange. In case you want to forcefully override an available bought price, you can apply `IGNORE_TRADES_BEFORE` and remove the pairs state json file after doing so.

{% hint style="info" %}
This parameter is irrelevant for margin trading
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a price per unit in base currency.

**Default value:** n/a
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Stop limit | RT buyback |
| DCA buy | Close |
| RT sell |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BOUGHT_PRICE`
{% endtab %}
{% endtabs %}

## Order type settings

On exchanges that support market orders, you can select which types of orders should be sent as limit or market.

{% hint style="success" %}
**Supported exchanges**

Binance, Bitfinex, Bitmex, Coinbase Pro, Kraken and Poloniex
{% endhint %}

### Market Buy

{% tabs %}
{% tab title="Description" %}
When set to true, strategy buy/long orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Stop limit |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_BUY`
{% endtab %}
{% endtabs %}

### Market Sell

{% tabs %}
{% tab title="Description" %}
When set to true, strategy sell/short orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Stop limit | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_SELL`
{% endtab %}
{% endtabs %}

### Market RT Buy

{% tabs %}
{% tab title="Description" %}
When set to true, RT\_BUY orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Stop limit |
|  | Strategy sell |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_RTBUY`
{% endtab %}
{% endtabs %}

### Market RT Sell

{% tabs %}
{% tab title="Description" %}
When set to true, RT\_SELL orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Stop limit |
|  | Strategy sell |
|  | RT buyback |
|  | RT buy |
|  | Close |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_RTSELL`
{% endtab %}
{% endtabs %}

### Market Buyback

{% tabs %}
{% tab title="Description" %}
When set to true, RT Buyback orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buyback | Stop limit |
|  | Strategy sell |
|  | RT sell |
|  | RT buy |
|  | Close |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_BUYBACK`
{% endtab %}
{% endtabs %}

### Market DU

{% tabs %}
{% tab title="Description" %}
When set to true, DU orders will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Stop limit |
|  | Strategy sell |
|  | RT sell |
|  | RT buy |
|  | Close |
|  | RT buyback |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_DU`
{% endtab %}
{% endtabs %}

### Market Close

{% tabs %}
{% tab title="Description" %}
When set to true, position close orders at Bitmex will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | Stop limit |
|  | Strategy sell |
|  | RT sell |
|  | RT buy |
|  | DCA buy |
|  | RT buyback |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_CLOSE`
{% endtab %}
{% endtabs %}

### Market Stop

{% tabs %}
{% tab title="Description" %}
When set to true, position stop orders at Bitmex will be placed as market order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Stop limit | Close |
|  | Strategy sell |
|  | RT sell |
|  | RT buy |
|  | DCA buy |
|  | RT buyback |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_STOP`
{% endtab %}
{% endtabs %}

### Market FOK

{% tabs %}
{% tab title="Description" %}
When set to true, FOK orders will be placed as market order. This refers to orders placed within `CANCEL_ORDERS_CYCLE_CAP`.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MARKET_FOK`
{% endtab %}
{% endtabs %}

