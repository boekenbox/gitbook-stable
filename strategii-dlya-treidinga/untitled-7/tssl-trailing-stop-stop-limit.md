# Tssl \(trailing stop / stop limit\)

This page describes how margin trading on Bitmex works with the TSSL strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy <a id="how-to-work-with-this-strategy"></a>

**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.

Using `tssl` \(margin\) is only meaningful with `MEAN_REVERSION`enabled.

The info below assumes you have set this.

The examples below show how the basic triggers for `tssl` work. Additionally, you can use confirming indicators and settings like ROE trailing.

### Long <a id="long"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-LnO40ymNOOfIwmU02OF%2F-LnO42-dZeeUTRKG5mWO%2Fimage.png?alt=media&token=de4a090b-1ff4-4891-9731-a3b1e3048d87)

* A long position is opened when buy trailing finishes below `LONG_LEVEL`.
* Position is closed when the desired `ROE` \(return on equity\) is reached. This is a percentage from the entry point, not taking leverage into consideration. Regardless what leverage is used, 1% price difference from your entry equals `ROE`: 1.
* A position is closed at loss when `STOP_BUY` is reached. This is a percentage from the entry point in the opposite direction of your profit target, not taking leverage into consideration. Regardless what leverage is used, 1% price difference from your entry equals `STOP_BUY`: 1.

### Short <a id="short"></a>

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-LnO4M8g6KlWsAs3lnsO%2F-LnO4NFmxIpa9uMSSR7w%2Fimage.png?alt=media&token=5064a06c-27fd-426d-af07-fb04816f5807)

* A short position is opened when sell trailing finishes above `SHORT_LEVEL`.
* Position is closed when the desired `ROE` \(return on equity\) is reached. This is a percentage from the entry point, not taking leverage into consideration. Regardless what leverage is used, 1% price difference from your entry equals `ROE`: 1.
* A position is closed at loss when `STOP_SELL` is reached. This is a percentage from the entry point in the opposite direction of your profit target, not taking leverage into consideration. Regardless what leverage is used, 1% price difference from your entry equals `STOP_SELL`: 1.

## Strategy parameters <a id="strategy-parameters"></a>

Following settings options are available for `tssl` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `tssl`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings <a id="margin-settings"></a>

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `tssl` as buy and/or sell method.

### Long Level <a id="long-level"></a>

This sets the target for opening a long position at a percentage above the highest EMA.

When you set this to 1, buy orders will only be placed when the current price is at least 1% above the currently highest EMA.

**Values:** numerical – represent a percentage.

**Default value:** 1

Parameter name in `config.js`: `LONG_LEVEL`

### Short Level <a id="short-level"></a>

This sets the target for opening a short position at a percentage below the lowest EMA.

When you set this to 1, sell orders will only be placed when the current price is at least 1% below the currently lowest EMA.

**Values:** numerical – represent a percentage.

**Default value:** 1

Parameter name in `config.js`: `SHORT_LEVEL`

### ROE <a id="roe"></a>

This sets the target for closing a position.

ROE is the Return On Equity for a position, the percentage profit and loss on your invested margin. This value is calculated in a similar way to how Bitmex calculates it, it does include leverage and does not include fees.

**Examples:** Long position, 1x leverage. When price moves 1% above the average entry price, 1% ROE is reached.

Long position, 100x leverage \(or cross leverage\). When price moves 1% above the average entry price, 100% ROE is reached.

Short position, 20x leverage When price moves 1% below the average entry price, 20% ROE is reached.

**Values:** numerical – represent a percentage.

**Default value:** 1

Parameter name in `config.js`: `ROE`

### Leverage <a id="leverage"></a>

Sets the leverage for opening any position. Setting 0 places the order with cross margin.

**Values:** numerical

**Default value:** 0

Parameter name in `config.js`: `LEVERAGE`

### Stop Buy <a id="stop-buy"></a>

Places a market stop order for a long position, at the same time as the position is opened.

When set to 1 and a long order is opened at a price of 100, a stop market order will be placed at 99.

**Values:** numerical - represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `STOP_BUY`

### Stop Sell <a id="stop-sell"></a>

Places a market stop order for a short position, at the same time as the position is opened.

When set to 1 and a short order is opened at a price of 100, a stop market order will be placed at 101.

**Values:** numerical - represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `STOP_SELL`

### ROE Trailing <a id="roe-trailing"></a>

Use this to enable tssl-style trailing for ROE.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `ROE_TRAILING`

### ROE Limit <a id="roe-limit"></a>

This sets the range for ROE trailing.

Setting a range of 5% at a ROE target of 1 would set an initial range between 0.95 and 1.05.

As long as ROE keeps increasing, the range moves along with ROE. As soon as ROE start decreasing, the lower range gets frozen. A close order is placed when ROE crosses the lower limit.

**Values:** numerical – represent a percentage of ROE.

**Default value:** 1

Parameter name in `config.js`: `ROE_LIMIT`

### Pre Order <a id="pre-order"></a>

When set to true, limit orders will placed at a configurable rate beyond the best bid/ask price to get ahead of the order book.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `PRE_ORDER`

### Pre Order Gap <a id="pre-order-gap"></a>

Sets the gap between the best bid/ask price in the orderbook and the rate at which a limit order gets placed. Long orders are placed at ask + gap. Short orders are placed at bid - gap.

It is possible to use negative values, this will increase the chance of receiving maker fees.

Example when set to 1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 101. When set to -1 and a buy signal occurs at an ask price of 100: a limit order gets placed at a rate of 99.

Don't use a negative gap together with `STOP_BUY` and/or `STOP_SELL`, as these stops do not combine well with position that do not always fill. Instead use `STOP_LIMIT`.

**Values:** numerical - represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `PRE_ORDER_GAP`

### Mean Reversion <a id="mean-reversion"></a>

When set to true, the strategy follows a mean reversion way of trading, instead of trend following.

Long and short levels are reversed in this mode, long level is placed below EMA, short level is placed above EMA.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MEAN_REVERSION`

## Buy settings <a id="buy-settings"></a>

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `tssl` as buy method.

### Buy enabled <a id="buy-enabled"></a>

Set this to false to prevent Gunbot from placing buy orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### Buy Range <a id="buy-range"></a>

This sets the buy range for trailing.

Setting a range of 0.5% at a starting price of 0.1 would set a range between 0.0995 and 0.1005.

As long as prices keep moving downwards, the range moves down along with the price. As soon as prices start going upward, the range freezes and a buy order is placed when the price crosses the upper boundary of the range.

**Values:** numerical, represents a percentage.

**Default value:** 0.5

Parameter name in `config.js`: `BUY_RANGE`

### NBA <a id="nba"></a>

"Never Buy Above". Use this to only allow buy orders below the last sell rate.

This sets the minimum percentage difference between the last sell order and the next buy. The default setting of 0 disables this option.

When set to 1, Gunbot will only place a buy order when the strategy buy criteria meet and price is at least 1% below the last sell price.

**Values:** numerical, represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `NBA`

## Sell settings <a id="sell-settings"></a>

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `tssl` as sell method.

### Sell enabled <a id="sell-enabled"></a>

Set this to false to prevent Gunbot from placing sell orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Sell Range <a id="sell-range"></a>

This sets the sell range for trailing. Setting a range of 0.5% at a starting price of 0.1 would set a range between 0.0995 and 0.1005.

As long as prices keep moving upwards, the range moves up along with the price. As soon as prices start going downward, the range freezes and a sell order is placed when the prices crosses the lower boundary of the range.

**Values:** numerical – represents a percentage.

**Default value:** 0.5

Parameter name in `config.js`: `SELL_RANGE`

## Indicator settings <a id="indicator-settings"></a>

Relevant indicators for trading with tssl.

These settings have a direct effect on trading with `tssl`, this is where you configure how Bollinger Bands are calculated and at which distance from them orders should be placed. Additionally, long and short level are dependent on EMA.

### Period <a id="period"></a>

This sets the candlestick period used for trading, this affects all indicators within the strategy.

Only use [supported values](https://wiki.gunthy.org/how-to-work-with-gunbot/basic-workings/period#supported-period-values).

Setting a short period allows you to trade on shorter trends, but be aware that these will be noisier than longer periods.

**Values:** numerical– represents candlestick size in minutes.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### Slow EMA <a id="slow-ema"></a>

Set this to the amount of candlesticks you want to use for your slow EMA. The closing price for each candle is used in the slow EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 2h for slow EMA – you need to set `EMA1` to 24 \(24 \* 5 mins\).

**Values:** numerical – represents a number of candlesticks.

**Default value:** 16

Parameter name in `config.js`: `EMA1`

### Fast EMA <a id="fast-ema"></a>

Set this to the amount of candlesticks you want to use for your fast EMA. The closing price for each candle is used in the fast EMA calculation.

For example: when you set `PERIOD` to 5, and want to use 1h for fast EMA – you need to set `EMA2` to 12 \(12 \* 5 mins\).

**Values:** numerical – represents a number of candlesticks.

**Default value:** 8

Parameter name in `config.js`: `EMA2`

## Balance settings <a id="balance-settings"></a>

## **Confirming indicators + advanced indicator settings** <a id="confirming-indicators-advanced-indicator-settings"></a>

## **Misc settings** <a id="misc-settings"></a>

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

DCA is not intented to be used for margin trading.

## Reversal trading settings <a id="reversal-trading-settings"></a>

RT is not intented to be used for margin trading.

## TrailMe settings <a id="trailme-settings"></a>

Because tssl is already a trailing strategy, you can't additionally use TrailMe.

## Placeholders <a id="placeholders"></a>

The following parameters in `config.js` have no function for this strategy and act as placeholder.

