# SMA cross

This page describes how margin trading works with the SMAcross strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy

{% hint style="info" %}
**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.
{% endhint %}

### Long / Buy

A long position is opened when `FAST_SMA` crosses over `SLOW_SMA`.

### Short / Sell

A short position is opened when `FAST_SMA` crosses down `SLOW_SMA`.

### Close

A position is closed when the desired `ROE` is reached.

### Stop

A position is closed at loss when `STOP_BUY` or `STOP_SELL` is reached.

## Strategy parameters

Following settings options are available for `SMACROSS` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `SMACROSS`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `SMACROSS` as buy and/or sell method.

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

### Leverage

{% tabs %}
{% tab title="Description" %}
Sets the leverage for opening any position. Setting 0 places the order with cross margin.
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

### ROE Limit

{% tabs %}
{% tab title="Description" %}
This sets the range for ROE trailing.

Setting a range of 5% at a ROE target of 1 would set an initial range between 0.95 and 1.05.

As long as ROE keeps increasing, the range moves along with ROE. As soon as ROE start decreasing, the lower range gets frozen. A close order is placed when ROE crosses the lower limit.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage of ROE.

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

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `SMACROSS` as buy method.

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

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `SMACROSS` as sell method.

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

Relevant indicators for trading with SMA cross.

These settings have a direct effect on trading with `SMACROSS`.

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

### Slow SMA

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating the slow SMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 2
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
Parameter name in `config.js`: `SLOW_SMA`
{% endtab %}
{% endtabs %}

### Fast SMA

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating the fast SMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 1
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
Parameter name in `config.js`: `FAST_SMA`
{% endtab %}
{% endtabs %}

## Balance settings

{% page-ref page="../balance-settings.md" %}

## **Confirming indicators + advanced indicator settings**

{% page-ref page="../confirming-indicators.md" %}

## **Misc settings**

{% page-ref page="../misc-settings.md" %}

## Dollar cost avg settings

DCA is not intented to be used for margin trading.

## Reversal trading settings

RT is not intented to be used for margin trading.

## TrailMe settings

TrailMe is not intended to be used with this strategy.

## Placeholders

The following parameters in `config.js` have no function for this strategy and act as placeholder.

| Parameter | Description |
| :--- | :--- |
| `ADX_ENABLED` | Placeholder. |
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
| `MACD_LONG` | Placeholder. |
| `MACD_SHORT` | Placeholder. |
| `MACD_SIGNAL` | Placeholder. |
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

