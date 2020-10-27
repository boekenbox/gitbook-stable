# Bollinger Bands

[Bollinger Bands](https://en.wikipedia.org/wiki/Bollinger_Bands) indicate relative high and low prices, using this information you can buy relatively low and sell relatively high.

With this strategy you can configure at which percentage from the lower Bollinger Band Gunbot should buy, and at which percentage from the upper Bollinger Band a sell order should be placed. Orders are placed as soon as price meets the set distance from the Bollinger Bands.

## Trading example

![](https://user-images.githubusercontent.com/2372008/47218525-504f2580-d3ac-11e8-86b6-eacc10011737.PNG)

_Example of how trading with this strategy can perform._ [_Details and settings_](https://www.tradingview.com/chart/BTCUSD/4v7xtwl5-Bollinger-Bands-Gunbot-trading-strategy/)_._

## How to work with this strategy

The infographic below describes what triggers trades with this strategy.

![](https://user-images.githubusercontent.com/2372008/40925551-4a0d684c-681a-11e8-8ea6-f0e6fba1af56.PNG)

_Note that this description is kept simple intentionally. In reality prices also need to be below_ `BUY_LEVEL` _before a buy is triggered._

\_\_

## Strategy parameters

Following settings options are available for `bb` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an override at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `bb`. Accepted values are all strategy names as listed [here](../about-gunbot-strategies/trading-methods.md).

\_\_

## Buy settings

Buy settings are the primary trigger for buy orders. These parameters control the execution of buy orders when using `ADX` as buy method.

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

### Buy Level

{% tabs %}
{% tab title="Description" %}
This sets the target for buying at a percentage below the lowest EMA. Additionally price must reach `LOW_BB`.

When you set this to 1, buy orders will only be placed when the current price is at least 1% below the currently lowest EMA.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

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
Parameter name in `config.js`: `BUY_LEVEL`
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

### Gain

{% tabs %}
{% tab title="Description" %}
This sets the minimum target for selling. Gunbot will sell once price reaches the set percentage above the break-even point. and `HIGH_BB` is reached.

If you want to have at least 2% profit per trade, set this to 2.
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
Parameter name in `config.js`: `GAIN`
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
This is an extra check that looks at your recent trading history to verify `GAIN` will be reached before placing a sell order.
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

Relevant indicators for trading with bb.

These settings have a direct effect on trading with `bb`.

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

### Low BB

{% tabs %}
{% tab title="Description" %}
This sets the target for buying. Negative values are allowed.

The bot will buy when price hits the set percentage from the lower Bollinger Band and the price is below the entry point as defined by `BUY_LEVEL`.

When set to 0, the lower Bollinger Band is the target. When set to 30, the target is 30% above the lower Bollinger Band - the upper band is at 100% from the lower band. Negative values are allowed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

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
|  | Strategy sell |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `LOW_BB`
{% endtab %}
{% endtabs %}

### High BB

{% tabs %}
{% tab title="Description" %}
This sets the target for selling. Negative values are allowed.

The bot will sell when price hits the set percentage from the upper Bollinger Band and `GAIN` is reached.

When set to 0, the upper Bollinger Band is the target \(well, almost\). When set to 30, the target is 30% under the upper Bollinger Band - the lower band is at 100% from the upper band. Negative values are allowed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| DCA buy \(when using HIGHBB option for DCA\) | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `HIGH_BB`
{% endtab %}
{% endtabs %}

### SMA Period

{% tabs %}
{% tab title="Description" %}
This defines the number of periods used for calculating Bollinger Bands.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a number of candlesticks.

**Default value:** 50
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| DCA buy \(when using HIGHBB option for DCA\) | RT sell |
|  | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SMAPERIOD`
{% endtab %}
{% endtabs %}

### Standard Deviation

{% tabs %}
{% tab title="Description" %}
This value defines the multiplier used for calculating Bollinger Bands.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical \(recommended: between 1.9 and 2.1\) - represents a multiplier value used in the bollinger bands calculation.

**Default value:** 2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| DCA buy \(when using HIGHBB option for DCA\) | RT sell |
|  | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STDV`
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

### Medium EMA

{% tabs %}
{% tab title="Description" %}
Set this to the amount of candlesticks you want to use for your medium EMA. The closing price for each candle is used in the fast EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 1h for medium EMA – you need to set `EMA2` to 12 \(12 \* 5 mins\).
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

## TrailMe settings

Parameters to configure additional trailing for various types of orders. Trailing works just like it does for the TSSL strategy, the difference being the starting point of trailing.

Orders resulting from trailing are only placed when the main strategy criteria are met, and confirming indicators \(if any\) allow the order. All these conditions must occur in the same cycle.

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
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LEVERAGE` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
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
| `SELLLVL1` | Placeholder. |
| `SELLLVL2` | Placeholder. |
| `SELLLVL3` | Placeholder. |
| `SELLLVL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

