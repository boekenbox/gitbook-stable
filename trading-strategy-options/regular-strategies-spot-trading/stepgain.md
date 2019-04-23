# Stepgain

Stepgain follows the markets prices and trend, and buys or sells as soon as the trend reverses. Assuming a trend reversal \(from downtrend to uptrend\) usually takes place close to the bottom of a price movement, this allows you to buy shortly after a price bottomed. Similarly, selling takes place when an uptrend turns into a downtrend.

Buying with Stepgain is based on [EMA](https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average), enabling Gunbot to start looking for a trend reversal after a set percentage below the lowest EMA is reached.

Trends are calculated automatically by XTrend and are visible in your logs. XTrend results can vary depending on your setting for `PERIOD`. Using XTrend is optional, you can also have stepgain trigger purely on price reversals above/below the configured level.

You can optionally use indicators like RSI or Stochastic as confirmation to only buy or sell when both a trend reversal and a specific indicator level occur.

## Trading example

![](https://user-images.githubusercontent.com/2372008/47218318-a66f9900-d3ab-11e8-858c-e4d2959a7371.PNG)

_Example of how trading with the stepgain strategy can perform._ [_Details and settings_](https://www.tradingview.com/chart/XLMBTC/SxHYdOCD-Stepgain-Gunbot-trading-strategy/)

## How to work with this strategy

The infographic below describes what triggers trades with this strategy.

![](https://user-images.githubusercontent.com/2372008/40718216-8e63121c-640f-11e8-917d-719104990195.PNG)

_In this example both BUYLVL and SELLLVL would be set to 2. A change of price movement direction is assumed a trend reversal in this example, in reality not every change of price direction will be considered a trend reversal because the strength of the trend is considered as well._

\_\_

## Strategy parameters

Following settings options are available for `stepgain` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an override at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `stepgain`. Accepted values are all strategy names as listed [here](../about-gunbot-strategies/trading-methods.md#available-buy-and-sell-methods).

## Buy settings

Buy settings are the primary trigger for buy orders. These parameters control the execution of buy orders when using `stepgain` as buy method.

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

### Stepgain Buy Lvl

{% tabs %}
{% tab title="Description" %}
This sets which step should be considered for buying:

1: Buy when price drops below `BUYLVL1` and the trend reverses or price hits `BUYLVL2`.

2: Buy when price drops below `BUYLVL2` and the trend reverses or price hits `BUYLVL3`.

3: Buy when price drops below `BUYLVL3` and the trend reverses.
{% endtab %}

{% tab title="Values" %}
**Values:** 1 / 2 / 3 – represents steps.

**Default value:** 1
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
Parameter name in `config.js`: `BUYLVL`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 1

{% tabs %}
{% tab title="Description" %}
Defines the first level below the lowest EMA to be considered for buying. Only used when `BUYLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 0.6
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
Parameter name in `config.js`: `BUYLVL1`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 2

{% tabs %}
{% tab title="Description" %}
Defines the second level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 2
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
Parameter name in `config.js`: `BUYLVL2`
{% endtab %}
{% endtabs %}

### Stepgain Buy Lvl 3

{% tabs %}
{% tab title="Description" %}
Defines the third level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent below the lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 70
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
Parameter name in `config.js`: `BUYLVL3`
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

### Take Buy

{% tabs %}
{% tab title="Description" %}
With this setting enabled, Gunbot will try to take any buy chance between the strategy entry point and your setting for `TBUY_RANGE`.

As soon as the ask price drops below the upper border of this range \(called "Take Buy"\), it will trail down with a range of `TBUY_RANGE` and place a buy order as soon as the ask price crosses up "Take Buy". Confirming indicators in use are respected.

Normal strategy buy orders are still possible while using `TAKE_BUY`.

This option should not be used together with reversal trading.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
Parameter name in `config.js`: `TAKE_BUY`
{% endtab %}
{% endtabs %}

### TBuy Range

{% tabs %}
{% tab title="Description" %}
This sets the buy range for `TAKE_BUY`.

When set to 0.5, the initial trailing stop is set 0.5% above the entry point defined by `BUY_LEVEL`.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

**Default value:** 0.5
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
Parameter name in `config.js`: `TBUY_RANGE`
{% endtab %}
{% endtabs %}

## Sell settings

Sell settings are the primary trigger for sell orders. These parameters control the execution of sell orders when using `bb` as sell method.

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

### Stepgain Sell Lvl

{% tabs %}
{% tab title="Description" %}
This sets which step should be considered for selling:

1: Sell when price rises above `SELLLVL1` and the trend reverses or price hits `SELLLVL2`.

2: Sell when price rises above `SELLLVL2` and the trend reverses or price hits `SELLLVL3`.

3: Sell when price rises above `SELLLVL3` and the trend reverses.
{% endtab %}

{% tab title="Values" %}
**Values:** 1 / 2 / 3 – represents steps.

**Default value:** 1
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
Parameter name in `config.js`: `SELLLVL`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 1

{% tabs %}
{% tab title="Description" %}
Defines the first level above the break-even point to be considered for selling. Only used when `SELLLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 0.6
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
Parameter name in `config.js`: `SELLLVL1`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 2

{% tabs %}
{% tab title="Description" %}
Defines the second level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 2
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
Parameter name in `config.js`: `SELLLVL2`
{% endtab %}
{% endtabs %}

### Stepgain Sell Lvl 3

{% tabs %}
{% tab title="Description" %}
Defines the third level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent above the average bought price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 70
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
Parameter name in `config.js`: `SELLLVL3`
{% endtab %}
{% endtabs %}

### Take Profit

{% tabs %}
{% tab title="Description" %}
With this setting enabled, Gunbot will try to take any possible profit between the break-even point and your strategy exit point. This can be useful, for example, on days where the markets move very slowly.

It works by trailing prices upwards between the break-even point and the strategy exit point, with a configurable range for trailing: `TP_RANGE`. A sell order will be placed when the trailing stop limit is hit or strategy sell conditions are reached. Confirming indicators in use are respected.

Sells at minimal loss are possible when using `TAKE_PROFIT`, acting as a sort of mini stop loss.

This option should not be used together with reversal trading or `DOUBLE_CHECK_GAIN`
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
|  |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TAKE_PROFIT`
{% endtab %}
{% endtabs %}

### TP Range

{% tabs %}
{% tab title="Description" %}
This sets the sell range for `TAKE_PROFIT`.

When set to 0.5, the initial trailing stop is set 0.5% below the break-even point.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 0.5
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
|  |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TP_RANGE`
{% endtab %}
{% endtabs %}

### TP Profit Only

{% tabs %}
{% tab title="Description" %}
Enable this to only allow sell orders above the break-even point.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
|  |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TP_PROFIT_ONLY`
{% endtab %}
{% endtabs %}

### Double Check Gain

{% tabs %}
{% tab title="Description" %}
This is an extra check that looks at your recent trading history to verify `SELLLVL` will be reached before placing a sell order.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
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
|  |  |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`
{% endtab %}
{% endtabs %}

## Indicator settings

Relevant indicators for trading with gain.

These settings have a direct effect on trading with `gain`, because `BUY_LEVEL` is dependant on EMA.

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
| DCA buy \(when using an indicator to trigger\) | RT sell |
|  | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `PERIOD`
{% endtab %}
{% endtabs %}

### Slow EMA

{% tabs %}
{% tab title="Description" %}
Set this to the amount of candlesticks you want to use for your slow EMA. The closing price for each candle is used in the slow EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 2h for slow EMA – you need to set `EMA1` to 24 \(24 \* 5 mins\).
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a number of candlesticks.

**Default value:** 16
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `EMA1`
{% endtab %}
{% endtabs %}

### Fast EMA

{% tabs %}
{% tab title="Description" %}
Set this to the amount of candlesticks you want to use for your fast EMA. The closing price for each candle is used in the fast EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 1h for fast EMA – you need to set `EMA2` to 12 \(12 \* 5 mins\).
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a number of candlesticks.

**Default value:** 8
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `EMA2`
{% endtab %}
{% endtabs %}

### XTrend Enabled

{% tabs %}
{% tab title="Description" %}
Disable this to use stepgain the legacy way: act on price reversals only - not considering XTrend.

When disabled, stepgain will trade when a price reversal above/below the set level happens.

When enabled, stepgain will trade when a price reversal above/below the set level happens and the direction of the trend is confirmed by XTrend.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `XTREND_ENABLED`
{% endtab %}
{% endtabs %}

## TrailMe settings

Parameters to configure additional trailing for various types of orders. Trailing works just like it does for the TSSL strategy, the difference being the starting point of trailing.

Orders resulting from trailing are only placed when the main strategy criteria are met, and confirming indicators \(if any\) allow the order. All these conditions must occur in the same cycle.

Because stepgain trades when trends reverse, it is not recommended to use additional trailing for normal buy or sell orders.

{% page-ref page="../trailme.md" %}

## Balance settings

{% page-ref page="../balance-settings.md" %}

## Confirming indicator + advanced indicator settings

{% page-ref page="../confirming-indicators.md" %}

## Dollar cost avg settings

{% page-ref page="../dollar-cost-avg-dca.md" %}

## Reversal trading settings

{% page-ref page="../reversal-trading-rt.md" %}

## Misc settings

{% page-ref page="../misc-settings.md" %}

## Placeholders

The following parameters in `config.js` have no function for this strategy and act as placeholder.

| Parameter | Description |
| :--- | :--- |
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `GAIN` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LEVERAGE` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
| `LOW_BB` | Placeholder. |
| `MACD_LONG` | Placeholder. |
| `MACD_SHORT` | Placeholder. |
| `MACD_SIGNAL` | Placeholder. |
| `MAKER_FEES` | Placeholder. |
| `MEAN_REVERSION` | Placeholder. |
| `PP_BUY` | Placeholder. |
| `PP_SELL` | Placeholder. |
| `PRE_ORDER_GAP` | Placeholder. |
| `PRE_ORDER` | Placeholder. |
| `RENKO_ATR` | Placeholder. |
| `RENKO_BRICK_SIZE` | Placeholder. |
| `RENKO_PERIOD` | Placeholder. |
| `ROE_CLOSE` | Placeholder. |
| `ROE_LIMIT` | Placeholder. |
| `ROE_TRAILING` | Placeholder. |
| `ROE` | Placeholder. |
| `RT_BUY_LEVEL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

