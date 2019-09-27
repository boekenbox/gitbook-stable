# MACD

This strategy is based on [MACD](https://www.investopedia.com/terms/m/macd.asp), taking advantage of signals that indicate momentum is likely changing.

A buy order is placed when the MACD line crosses up the signal line, a sell order is placed when the MACD line crosses down the signal line.

To refine this strategy, other indicators are available to be used as confirmation for both buying and selling. For example you could have Gunbot buy on a bullish signal and RSI is 30 or lower.

{% hint style="warning" %}
Gain protection is optional for this strategy.

Be aware that this can lead to sell orders below your break-even point.

Normal constraints like "buy once, sell once" don't apply to this strategy: each time buying criteria occur, Gunbot will place a buy order as high as set in your balance settings.
{% endhint %}

### Trading example

![](https://user-images.githubusercontent.com/2372008/47229545-41766c00-d3c8-11e8-989f-67ab1b50b653.PNG)

_Example of how trading with this strategy can perform._ [_Details and settings_](https://www.tradingview.com/chart/ETHUSD/1mG1CbWo-MACD-Gunbot-trading-strategy/)

## How to work with this strategy

The infographic below describes what triggers trades with this strategy.

![](https://user-images.githubusercontent.com/2372008/41107283-5ba76206-6a72-11e8-88ef-ecf39e1ce953.PNG)

## Strategy parameters

Following settings options are available for `MACD` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an override at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `MACD`. Accepted values are all strategy names as listed [here](../about-gunbot-strategies/trading-methods.md#available-buy-and-sell-methods).

## Buy settings

Buy settings are the primary trigger for buy orders. These parameters control the execution of buy orders when using `MACD` as buy method.

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

### Single Buy

{% tabs %}
{% tab title="Description" %}
Enable this to restrict `MACD` to placing just a single buy order on the first crossup, then it will go in sell mode.

Useful when you combine `MACD` with a different sell method or use other parameters that prevents sell orders from happening at every crossdown.
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
Parameter name in `config.js`: `SINGLE_BUY`
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

### Buy Level

{% tabs %}
{% tab title="Description" %}
This sets entry point for `TAKE_BUY` at a percentage below the lowest EMA.

When you set this to 1, the entry point will be set 1% below the currently lowest EMA.
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

## Sell settings

Sell settings are the primary trigger for sell orders. These parameters control the execution of sell orders when using `MACD` as sell method.

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
Disable this to allow sell orders below the break-even point.
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

### Gain

{% tabs %}
{% tab title="Description" %}
This sets the minimum target for selling when `DOUBLE_CHECK_GAIN` is enabled.
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

## Indicator settings

Relevant indicators for trading with MACD.

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
| DCA buy \(when using an indicator to trigger\) | RT sell |
|  | Close |
|  | Stop limit |
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

## TrailMe settings

Parameters to configure additional trailing for various types of orders. Trailing works just like it does for the TSSL strategy, the difference being the starting point of trailing.

Orders resulting from trailing are only placed when the main strategy criteria are met, and confirming indicators \(if any\) allow the order. All these conditions must occur in the same cycle.

Because this strategy trades on crosses happening in a single cycle, it is not recommended to use additional trailing for normal buy and sell orders. Trades will only happen when both the trailing stop and strategy cross happen in the same round.

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

