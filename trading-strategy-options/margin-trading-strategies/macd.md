# MACD

This page describes how margin trading works with the [MACD](https://github.com/GuntharDeNiro/BTCT/wiki/MACD) strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy

{% hint style="info" %}
**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.
{% endhint %}

### Long / Buy

A long position is opened when MACD crosses up the signal line.

### Short / Sell

A short position is opened when MACD crosses down the signal line.

### Close

A position is closed when the desired `ROE` is reached.

### Stop

A position is closed at loss when its stop is reached.

## Strategy parameters

Following settings options are available for `MACD` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `MACD`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `MACD` as buy and/or sell method.

### ROE

{% tabs %}
{% tab title="Description" %}
This sets the target for closing a position.

ROE is the Return On Equity for a position, the percentage profit and loss on your invested margin. This value is calculated in a similar way to how Bitmex calculates it, it does include leverage and does not include fees.

**Examples:**

Long position, 1x leverage.  
When price moves 1% above the average entry price, 1% ROE is reached.

Long position, 100x leverage \(or cross leverage\).  
When price moves 1% above the average entry price, 100% ROE is reached.

Short position, 20x leverage  
When price moves 1% below the average entry price, 20% ROE is reached.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE`
{% endtab %}
{% endtabs %}

### PND

{% tabs %}
{% tab title="Description" %}
Use "PND" logic to close trades. This mode tries to not close a position before a pump or dump has fully played out - usually beats ROE trailing performance.

Respects the minimum ROE set.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PND`
{% endtab %}
{% endtabs %}

### PND protection

{% tabs %}
{% tab title="Description" %}
Threshold to close a position when it drops below ROE again.

A value of 1.5 means that if ROE reached 1.5x the minimum target, the position will get closed immediately if the trend turns.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 1.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PND_PROTECTION`
{% endtab %}
{% endtabs %}

### Leverage

{% tabs %}
{% tab title="Description" %}
Sets the leverage for opening any position. Setting 0 places the order with cross margin.

{% hint style="warning" %}
On Binance Futures you must set leverage per pair on the exchange itself.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
| Strategy sell | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `LEVERAGE`
{% endtab %}
{% endtabs %}

### Stop Buy

{% tabs %}
{% tab title="Description" %}
Places a market stop order for a long position, at the same time as the position is opened.

When set to 1 and a long order is opened at a price of 100, a stop market order will be placed at 99.

{% hint style="info" %}
This setting is exclusive to Bitmex
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOP_BUY`
{% endtab %}
{% endtabs %}

### Stop Sell

{% tabs %}
{% tab title="Description" %}
Places a market stop order for a short position, at the same time as the position is opened.

When set to 1 and a short order is opened at a price of 100, a stop market order will be placed at 101.

{% hint style="info" %}
This setting is exclusive to Bitmex
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOP_SELL`
{% endtab %}
{% endtabs %}

### ROE Trailing

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for ROE.

Trailing limit is set with `ROE_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_TRAILING`
{% endtab %}
{% endtabs %}

### ROE Scalper

{% tabs %}
{% tab title="Description" %}
Use this to enable an alternate trailing mechanism for closing positions.

Trailing limit is set with `ROE_LIMIT`. Additionally `ROE_TRAILING` must be enabled.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_SCALPER`
{% endtab %}
{% endtabs %}

### ROE Limit

{% tabs %}
{% tab title="Description" %}
This sets the range for ROE trailing.

**ROE trailing:**

Range is a percentage of current ROE. Setting a `ROE_LIMIT` of 5 at a `ROE` target of 1 would set an initial range between 0.95 and 1.05.

**ROE scalper:**

Range is an absolute ROE value. Setting a ROE\_LIMIT of 5 at a `ROE` target of 10 means that the trailing stop is initially set at ROE 5 \(`ROE` minus `ROE_LIMIT`\).

**Both**:

As long as ROE keeps increasing, the range moves along with ROE. As soon as ROE start decreasing, the lower range gets frozen. A close order is placed when ROE crosses the lower limit.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a trailing range.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy sell |
|  | Stop limit |
|  | Close |
|  | Strategy buy |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_LIMIT`
{% endtab %}
{% endtabs %}

### Pre Order

{% tabs %}
{% tab title="Description" %}
When set to true, limit orders will placed at a configurable rate beyond the best bid/ask price to get ahead of the order book.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | RT buy |
| Strategy sell | RT buyback |
| Strategy buy | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PRE_ORDER`
{% endtab %}
{% endtabs %}

### Pre Order Gap

{% tabs %}
{% tab title="Description" %}
Sets the gap between the best bid/ask price in the orderbook and the rate at which a limit order gets placed. Long orders are placed at ask + gap. Short orders are placed at bid - gap.

It is possible to use negative values, this will increase the chance of receiving maker fees.

Example when set to 1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 101. When set to -1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 99.

Don't use a negative gap together with `STOP_BUY` and/or `STOP_SELL`, as these stops do not combine well with position that do not always fill. Instead use `STOP_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Stop limit |
|  | DCA buy |
|  | Close |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PRE_ORDER_GAP`
{% endtab %}
{% endtabs %}

## Buy settings

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `MACD` as buy method.

### Buy enabled

{% tabs %}
{% tab title="Description" %}
Set this to false to prevent Gunbot from placing buy orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
| DCA buy | Stop limit |
| RT buy | Close |
| RT buyback | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `BUY_ENABLED`
{% endtab %}
{% endtabs %}

### NBA

{% tabs %}
{% tab title="Description" %}
"Never Buy Above". Use this to only allow buy orders below the last sell rate.

This sets the minimum percentage difference between the last sell order and the next buy. The default setting of 0 disables this option.

When set to 1, Gunbot will only place a buy order when the strategy buy criteria meet and price is at least 1% below the last sell price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | Strategy sell |
|  | Stop limit |
|  | Close |
|  | RT sell |
|  | DCA buy |
|  | RT buy |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `NBA`
{% endtab %}
{% endtabs %}

## Sell settings

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `MACD` as sell method.

### Sell enabled

{% tabs %}
{% tab title="Description" %}
Set this to false to prevent Gunbot from placing sell orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
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
Parameter name in `config.js`: `SELL_ENABLED`
{% endtab %}
{% endtabs %}

## Indicator settings

Relevant indicators for trading with EMA spread.

These settings have a direct effect on trading with `MACD`.

### Period

{% tabs %}
{% tab title="Description" %}
This sets the candlestick period used for trading, this affects all indicators within the strategy.

Only use [supported values](../../how-to-work-with-gunbot/basic-workings/period.md#supported-period-values).

Setting a short period allows you to trade on shorter trends, but be aware that these will be noisier than longer periods.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical– represents candlestick size in minutes.

**Default value:** 15
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PERIOD`
{% endtab %}
{% endtabs %}

### MACD Short

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating the short-term moving average for MACD.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MACD_SHORT`
{% endtab %}
{% endtabs %}

### MACD Long

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating the long-term moving average for MACD.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 20
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MACD_LONG`
{% endtab %}
{% endtabs %}

### MACD Signal

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating the MACD signal line.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 10
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MACD_SIGNAL`
{% endtab %}
{% endtabs %}

## Balance settings

{% page-ref page="../balance-settings.md" %}

## **Confirming indicators + advanced indicator settings**

{% page-ref page="../confirming-indicators.md" %}

## **Misc settings**

{% page-ref page="../misc-settings.md" %}

## Dollar cost avg settings

### DCA Method

{% tabs %}
{% tab title="Description" %}
When set to **TENKAN** Gunbot will place DCA orders in exactly the same way as in the [Tenkan ](tenkan.md)strategy. Each DCA order is in size of `TRADING_LIMIT`.
{% endtab %}

{% tab title="Values" %}
**Values:** NATIVE / TENKAN

**Default value:** TENKAN   
  
Setting NATIVE effectively disables DCA in this strategy
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA  | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DCA_METHOD`
{% endtab %}
{% endtabs %}

## Reversal trading settings

RT is not intented to be used for margin trading.

## TrailMe settings

TrailMe is not intended to be used with this strategy.

## Placeholders

The following parameters in `config.js` have no function for this strategy and act as placeholder.

| Parameter | Description |
| :--- | :--- |
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `DOUBLE_CHECK_GAIN` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `GAIN` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_BUY` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_SELL` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_BUY` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SELL` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
| `LOW_BB` | Placeholder. |
| `MEAN_REVERSION` | Placeholder. |
| `PP_BUY` | Placeholder. |
| `PP_SELL` | Placeholder. |
| `RENKO_ATR` | Placeholder. |
| `RENKO_BRICK_SIZE` | Placeholder. |
| `RENKO_PERIOD` | Placeholder. |
| `SELLLVL1` | Placeholder. |
| `SELLLVL2` | Placeholder. |
| `SELLLVL3` | Placeholder. |
| `SELLLVL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TAKE_BUY` | Placeholder. |
| `TBUY_RANGE` | Placeholder. |
| `TENKAN_BUY` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_SELL` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TP_PROFIT_ONLY` | Placeholder. |
| `TP_RANGE` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

