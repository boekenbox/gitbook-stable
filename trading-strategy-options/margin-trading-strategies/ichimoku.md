# Ichimoku

This page describes how margin trading works with the [ichimoku](https://github.com/GuntharDeNiro/BTCT/wiki/Ichimoku) strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy

{% hint style="info" %}
**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.
{% endhint %}



For this strategy it is recommended to use an additional momentum indicator to confirm long and short entries.

### Long / Buy

Before a long position is opened, one of the following conditions must occur to put Gunbot on "long alert":

1. The current candle crosses over Kumo. This means that all of open, close, high and low must be above Kumo.
2. Tenkan-sen crosses over Kijun-sen, above Kumo.

A long position is then opened when both of the following confirmations happens in a later cycle, they do not have to happen in the same cycle:

1. Chikou-span crosses over the "past Kumo" \(Kumo value of x periods ago, defined by `DISPLACEMENT`\).
2. The "future Kumo" is bullish/green \(Kumo value of x periods in the future, defined by `DISPLACEMENT`\).

### Short / Sell

Before a short position is opened, one of the following conditions must occur to put Gunbot on "short alert":

1. The current candle crosses below Kumo. This means that all of open, close, high and low must be below Kumo.
2. Tenkan-sen crosses under Kijun-sen, below Kumo.

A short position is then opened when both of the following confirmations happens in a later cycle, they do not have to happen in the same cycle:

1. Chikou-span crosses under the "past Kumo" \(Kumo value of x periods ago, defined by `DISPLACEMENT`\).
2. The "future Kumo" is bearish/red \(Kumo value of x periods in the future, defined by `DISPLACEMENT`\).

### Close

A long position is closed when the current candle crosses under Tenkan-sen, Kijun-sen or Kumo. This means that all of open, close, high and low must be below the selected item. Alternatively, you can set a ROE target for closing a position.

A short position is closed when the current candle crosses over Tenkan-sen, Kijun-sen or Kumo. This means that all of open, close, high and low must be above the selected item. Alternatively, you can set a ROE target for closing a position.

You can configure which of the three items is used for closing a position, with `TENKAN_CLOSE`, `KIJUN_CLOSE`, `KUMO_CLOSE` or `ROE_CLOSE`. If multiple of these parameters are set to true, the first of which occurs will close the position.

### Stop

A long position is stopped when the current candle crosses under Tenkan-sen, Kijun-sen or Kumo. This means that all of open, close, high and low must be above the selected item.

A short position is stopped when the current candle crosses over Tenkan-sen, Kijun-sen or Kumo. This means that all of open, close, high and low must be over the selected item.

You can configure which of the three items is used for stopping a position, with `TENKAN_STOP`, `KIJUN_STOP` or `KUMO_STOP`. If multiple of these parameters are set to true, the first of which occurs will close the position. Make sure to use different lines for closing and stopping a position.

After a stop is hit, the "alert" conditions for a long or short must happen again before another position is opened.

### Strategy parameters

Following settings options are available for `ichimoku` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `ichimoku`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `ichimoku` as buy and/or sell method.

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

Don't use a negative gap together with `STOP_BUY` and/or `STOP_SELL`, as these stops do not combine well with position that do not always fill. 
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

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `ichimoku` as buy method.

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

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `ichimoku` as sell method.

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

These settings have a direct effect on trading with `ichimoku`.

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

### Tenkan Period

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating Tenkan-sen.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 9
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TENKAN_PERIOD`
{% endtab %}
{% endtabs %}

### Kijun Period

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating Kijun-sen.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 26
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KIJUN_PERIOD`
{% endtab %}
{% endtabs %}

### Senkouspan Period

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating Senkou span.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 52
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SENKOUSPAN_PERIOD`
{% endtab %}
{% endtabs %}

### Displacement

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for displacing Kumo and Chikou-span.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 26
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DISPLACEMENT`
{% endtab %}
{% endtabs %}

### Kumo Close

{% tabs %}
{% tab title="Description" %}
Enable this to close a position when the current candle moves completely below \(long\) or above \(short\) Kijun-sen.

Do not enable multiple close triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KUMO_CLOSE`
{% endtab %}
{% endtabs %}

### Tenkan Close

{% tabs %}
{% tab title="Description" %}
Enable this to close a position when the current candle moves completely below \(long\) or above \(short\) Tenkan-sen.

Do not enable multiple close triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TENKAN_CLOSE`
{% endtab %}
{% endtabs %}

### Kijun Close

{% tabs %}
{% tab title="Description" %}
Enable this to close a position when the current candle moves completely below \(long\) or above \(short\) Kijun-sen.

Do not enable multiple close triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KIJUN_CLOSE`
{% endtab %}
{% endtabs %}

### ROE Close

{% tabs %}
{% tab title="Description" %}
Enable this to stop a position when ROE is reached.

Do not enable multiple close triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `ROE_CLOSE`
{% endtab %}
{% endtabs %}

### Kumo Stop

{% tabs %}
{% tab title="Description" %}
Enable this to stop a position when the current candle moves completely below \(long\) or above \(short\) Kumo.

Do not enable multiple stop triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KUMO_STOP`
{% endtab %}
{% endtabs %}

### Tenkan Stop

{% tabs %}
{% tab title="Description" %}
Enable this to close a position when the current candle moves completely below \(long\) or above \(short\) Tenkan-sen.

Do not enable multiple stop triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TENKAN_STOP`
{% endtab %}
{% endtabs %}

### Kijun Stop

{% tabs %}
{% tab title="Description" %}
Enable this to stop a position when the current candle moves completely below \(long\) or above \(short\) Kijun-sen.

Do not enable multiple stop triggers.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Close | Strategy sell |
|  | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Strategy buy |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `KIJUN_STOP`
{% endtab %}
{% endtabs %}

### Ichimoku Protection

{% tabs %}
{% tab title="Description" %}
Enable this to only allow trades when they are confirmed by the cloud. Set this to false to ignore Kumu trends.

When enabled, buy orders can only happen above Kumo, sell orders can only happen below Kumo.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
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
Parameter name in `config.js`: `ICHIMOKU_PROTECTION`
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
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DOUBLE_CHECK_GAIN` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `GAIN` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_BUY` | Placeholder. |
| `KIJUN_SELL` | Placeholder. |
| `KUMO_BUY` | Placeholder. |
| `KUMO_SELL` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
| `LOW_BB` | Placeholder. |
| `MACD_LONG` | Placeholder. |
| `MACD_SHORT` | Placeholder. |
| `MACD_SIGNAL` | Placeholder. |
| `MEAN_REVERSION` | Placeholder. |
| `PP_BUY` | Placeholder. |
| `PP_SELL` | Placeholder. |
| `SELLLVL1` | Placeholder. |
| `SELLLVL2` | Placeholder. |
| `SELLLVL3` | Placeholder. |
| `SELLLVL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `STOP_LIMIT` | Placeholder. |
| `TAKE_BUY` | Placeholder. |
| `TBUY_RANGE` | Placeholder. |
| `TENKAN_BUY` | Placeholder. |
| `TENKAN_SELL` | Placeholder. |
| `TP_PROFIT_ONLY` | Placeholder. |
| `TP_RANGE` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |

