# Stepgain

This page describes how margin trading on Bitmex works with the Stepgain strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy <a id="how-to-work-with-this-strategy"></a>

**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.

Using `stepgain` \(margin\) is only meaningful with `MEAN_REVERSION` enabled.

The info below assumes you have set this.

### Long / Buy <a id="long-buy"></a>

A long position is opened when price is between your stepgain buy levels and changes direction while being below `LONG_LEVEL`.

### Short / Sell <a id="short-sell"></a>

A short position is opened when price is between your stepgain sell levels and changes direction while being above `SHORT_LEVEL`.

### Close <a id="close"></a>

A position is closed when the desired `ROE` is reached.

### Stop <a id="stop"></a>

A position is closed at loss when `STOP_BUY` or `STOP_SELL` is reached.

## Strategy parameters <a id="strategy-parameters"></a>

Following settings options are available for `stepgain` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `stepgain`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings <a id="margin-settings"></a>

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `stepgain` as buy and/or sell method.

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

This setting must be enabled to use stepgain as buy or sell method. When set to false, this strategy reverts to the same behavior as the gain strategy.

Long and short levels are reversed in this mode, long level is placed below EMA, short level is placed above EMA.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MEAN_REVERSION`

## Buy settings <a id="buy-settings"></a>

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `stepgain` as buy method.

### Buy enabled <a id="buy-enabled"></a>

Set this to false to prevent Gunbot from placing buy orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### Stepgain Buy Lvl <a id="stepgain-buy-lvl"></a>

This sets which step should be considered for buying:

1: Buy when price drops below `BUYLVL1` and the trend reverses or price hits `BUYLVL2`.

2: Buy when price drops below `BUYLVL2` and the trend reverses or price hits `BUYLVL3`.

3: Buy when price drops below `BUYLVL3` and the trend reverses.

**Values:** 1 / 2 / 3 – represents steps.

**Default value:** 1

Parameter name in `config.js`: `BUYLVL`

### Stepgain Buy Lvl 1 <a id="stepgain-buy-lvl-1"></a>

Defines the first level below the lowest EMA to be considered for buying. Only used when `BUYLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent below the lowest EMA.

**Values:** numerical – represents a percentage.

**Default value:** 0.6

Parameter name in `config.js`: `BUYLVL1`

### Stepgain Buy Lvl 2 <a id="stepgain-buy-lvl-2"></a>

Defines the second level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent below the lowest EMA.

**Values:** numerical – represents a percentage.

**Default value:** 2

Parameter name in `config.js`: `BUYLVL2`

### Stepgain Buy Lvl 3 <a id="stepgain-buy-lvl-3"></a>

Defines the third level below the lowest EMA to be considered for buying. Used when `BUYLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent below the lowest EMA.

**Values:** numerical – represents a percentage.

**Default value:** 70

Parameter name in `config.js`: `BUYLVL3`

### NBA <a id="nba"></a>

"Never Buy Above". Use this to only allow buy orders below the last sell rate.

This sets the minimum percentage difference between the last sell order and the next buy. The default setting of 0 disables this option.

When set to 1, Gunbot will only place a buy order when the strategy buy criteria meet and price is at least 1% below the last sell price.

**Values:** numerical, represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `NBA`

## Sell settings <a id="sell-settings"></a>

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `stepgain` as sell method.

### Sell enabled <a id="sell-enabled"></a>

Set this to false to prevent Gunbot from placing sell orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Stepgain Sell Lvl <a id="stepgain-sell-lvl"></a>

This sets which step should be considered for selling:

1: Sell when price rises above `SELLLVL1` and the trend reverses or price hits `SELLLVL2`.

2: Sell when price rises above `SELLLVL2` and the trend reverses or price hits `SELLLVL3`.

3: Sell when price rises above `SELLLVL3` and the trend reverses.

**Values:** 1 / 2 / 3 – represents steps.

**Default value:** 1

Parameter name in `config.js`: `SELLLVL`

### Stepgain Sell Lvl 1 <a id="stepgain-sell-lvl-1"></a>

Defines the first level above the break-even point to be considered for selling. Only used when `SELLLVL` is set to 1.

When set to 1, this means that the price needs to be at least 1 percent above the average bought price.

**Values:** numerical – represents a percentage.

**Default value:** 0.6

Parameter name in `config.js`: `SELLLVL1`

### Stepgain Sell Lvl 2 <a id="stepgain-sell-lvl-2"></a>

Defines the second level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 1 or 2.

When set to 2, this means that the price needs to be at least 2 percent above the average bought price.

**Values:** numerical – represents a percentage.

**Default value:** 2

Parameter name in `config.js`: `SELLLVL2`

### Stepgain Sell Lvl 3 <a id="stepgain-sell-lvl-3"></a>

Defines the third level above the break-even point to be considered for selling. Used when `SELLLVL` is set to 2 or 3.

When set to 10, this means that the price needs to be at least 10 percent above the average bought price.

**Values:** numerical – represents a percentage.

**Default value:** 70

Parameter name in `config.js`: `SELLLVL3`

## Indicator settings <a id="indicator-settings"></a>

Relevant indicators for trading with stepgain.

These settings have a direct effect on trading with `stepgain`, this is where you configure the EMAs.

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

With margin trading, additional trailing only works when MEAN\_REVERSION is enabled.

Parameters to configure additional trailing for various types of orders. Trailing works just like it does for the TSSL strategy, the difference being the starting point of trailing.

Orders resulting from trailing are only placed when the main strategy criteria are met, and confirming indicators \(if any\) allow the order. All these conditions must occur in the same cycle.

## Placeholders <a id="placeholders"></a>

The following parameters in `config.js` have no function for this strategy and act as placeholder.

