---
description: Options that define how much Gunbot is allowed to spend.
---

# Balance settings

Balance settings define important options like how much Gunbot is allowed to invest per trade.

These settings are part of a trading strategy, this way you have the flexibility to run different strategies with different balance settings.

## Balance settings parameters

### Trading Limit

{% tabs %}
{% tab title="Description" %}
This value defines the trading limit for each buy order. Make sure to always set this higher than `MIN_VOLUME_TO_BUY` & `MIN_VOLUME_TO_SELL`.

When you set this to 0.1 and trade BTC-x pairs, Gunbot will place a buy order worth 0.1 BTC each time it buys.

When trading a fiat pair like USD-x, set a whole number like 100 as trading limit.

{% hint style="info" %}
Bitmex: enter the desired number of contracts.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.002
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRADING_LIMIT`
{% endtab %}
{% endtabs %}

### Trading Fees

{% tabs %}
{% tab title="Description" %}
This sets the trading fees paid to the exchange. Gunbot uses this data to calculate the break-even point.

Does your exchange charge 0.25% fees per trade? Then set this to 0.25. When your exchange has different fees for different types of trades, set the average fees per trade.

Trading fees are reflected in the average bought price. Exchanges only calculate fees after the trade comes in, Gunbot needs to know about fees before the trade is sent to the exchange.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage

**Default value:** 0.25
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Stop limit | RT buy |
| RT buyback | RT sell |
|  | DCA buy |
|  | Close |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRADING_FEES`
{% endtab %}
{% endtabs %}

### Trading Limit Percentage

{% tabs %}
{% tab title="Description" %}
Alternative method for setting the investment per buy order as a percentage of the available base currency at the time the trade takes place.

Any value above 0 makes Gunbot ignore `TRADING_LIMIT` and uses the set percentage instead. For example: when set to 10, trading BTC-ALT and you have 1 BTC available at the time Gunbot places a buy order, an order of 0.1 BTC is placed.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage of available base currency.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TL_PERC`
{% endtab %}
{% endtabs %}

### Trading Limit All-In

{% tabs %}
{% tab title="Description" %}
Alternative method for setting the investment per buy order to use all available base currency at the time the trade takes place.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TL_ALLIN`
{% endtab %}
{% endtabs %}

### Keep Quote

{% tabs %}
{% tab title="Description" %}
Sets an amount of quote currency that will not be sold. For a BTC-ALT pair, a set amount of ALT will not be sold.

For example: when `KEEP_QUOTE` is set to 10 for trading BTC-BNB, then Gunbot will leave 10 BNB in your account when placing a sell order, any balance above 10 will get sold as long as it is worth more than `MIN_VOLUME_TO_SELL`.

To make sure trading continues after a sell order where an amount of quote is kept, make sure to set `MIN_VOLUME_TO_SELL` at least higher than the assumed value of the kept quote \(expressed in base currency\). When you do not do this, and set `MIN_VOLUME_TO_SELL` lower than the value of quote kept, Gunbot will attempt to sell again after the initial sell order \(as balance is higher than `MIN_VOLUME_TO_SELL`\) - which won't succeed since you only own the amount specified in `KEEP_QUOTE`.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents an amount in quote currency.

**Default value:** 0
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KEEP_QUOTE`
{% endtab %}
{% endtabs %}

### Funds Reserve

{% tabs %}
{% tab title="Description" %}
Sets an amount of base currency that will not be traded. For a BTC-x pair, funds in BTC would be reserved according to this setting. For ETH- pairs ETH would be reserved, etc. It is recommended to use the same value for all pairs of the same base currency you run.

For example: when funds reserve is set to 0.5 for trading BTC-x pairs then 0.5 BTC will not be used by Gunbot.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.0001
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `FUNDS_RESERVE`
{% endtab %}
{% endtabs %}

### Min Volume To Buy

{% tabs %}
{% tab title="Description" %}
Sets a threshold for buy orders. Prevents orders below the exchange minimum trade size from being placed.

Set this at least to the minimum trade size of your exchange.

When trading a fiat pair like USD-x, set a whole number like 10 as `MIN_VOLUME_TO_BUY`.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents volume in base currency

**Default value:** 0.001
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
| DCA buy | RT sell |
| RT buy | RT buyback |
| RT buyback | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MIN_VOLUME_TO_SELL`
{% endtab %}
{% endtabs %}

### Min Volume To Sell

{% tabs %}
{% tab title="Description" %}
Sets a threshold for sell orders lower than the exchange minimum trade size.

If you own less than the set amount, sell orders will not be placed and Gunbot goes into buying mode. Set this at least to the minimum trade size of your exchange.

When trading a fiat pair like USDT-x, set a whole number like 10 as `MIN_VOLUME_TO_SELL`.

When you hold 0.006 \(in base currency\) of a coin and have set `MIN_VOLUME_TO_SELL` to 0.01, Gunbot will not try to sell your current 0.006 balance because it is below the set threshold of 0.01, instead it will place another buy order first as soon as buying are met.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents volume in base currency

**Default value:** 0.001
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Stop limit | RT buy |
| RT sell | RT buyback |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MIN_VOLUME_TO_BUY`
{% endtab %}
{% endtabs %}

