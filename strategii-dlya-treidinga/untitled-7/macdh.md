# MACDH

This page describes how margin trading on Bitmex works with the MACDH strategy. The triggers for trades are slightly different than in the same strategy for regular trading.

## How to work with this strategy <a id="how-to-work-with-this-strategy"></a>

**Expected behavior for margin trading**

Gunbot will open one position, either long or short, and close this position when the target is reached. When the stop is hit before profitably closing a trade, Gunbot will place a stop order at loss. After closing a position, Gunbot will again look to open a new long or short position. Gunbot will not add to existing open positions.

Please don't manually add to or reduce positions opened by Gunbot, unless you stop running Gunbot on this trading pair until you've closed this position.

​

### Long / Buy <a id="long-buy"></a>

A long position is opened when MACDH crosses over zero.

### Short / Sell <a id="short-sell"></a>

A short position is opened when MACDH crosses under zero.

### Close <a id="close"></a>

A position is closed when the desired `ROE` is reached.

### Stop <a id="stop"></a>

A position is closed at loss when `STOP_BUY` or `STOP_SELL` is reached.

## Strategy parameters <a id="strategy-parameters"></a>

Following settings options are available for `MACDH` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an [override](https://github.com/GuntharDeNiro/BTCT/wiki/Gunbot-settings#overrides) at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `MACDH`. Accepted values are all strategy names as listed [here](https://github.com/GuntharDeNiro/BTCT/wiki/About-Gunbot-strategies).

## Margin settings <a id="margin-settings"></a>

Margin settings control settings like leverage and the target for ROE. These parameters are relevant when using `MACDH` as buy and/or sell method.

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

## Buy settings <a id="buy-settings"></a>

Buy settings are the primary trigger for opening long positions. These parameters control the execution of buy orders when using `MACDH` as buy method.

### Buy enabled <a id="buy-enabled"></a>

Set this to false to prevent Gunbot from placing buy orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### NBA <a id="nba"></a>

"Never Buy Above". Use this to only allow buy orders below the last sell rate.

This sets the minimum percentage difference between the last sell order and the next buy. The default setting of 0 disables this option.

When set to 1, Gunbot will only place a buy order when the strategy buy criteria meet and price is at least 1% below the last sell price.

**Values:** numerical, represents a percentage.

**Default value:** 0

Parameter name in `config.js`: `NBA`

## Sell settings <a id="sell-settings"></a>

Sell settings are the primary trigger for opening short positions. These parameters control the execution of sell orders when using `MACD` as sell method.

### Sell enabled <a id="sell-enabled"></a>

Set this to false to prevent Gunbot from placing sell orders.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

## Indicator settings <a id="indicator-settings"></a>

Relevant indicators for trading with EMA spread.

These settings have a direct effect on trading with `MACDH`.

### Period <a id="period"></a>

This sets the candlestick period used for trading, this affects all indicators within the strategy.

Only use [supported values](https://wiki.gunthy.org/how-to-work-with-gunbot/basic-workings/period#supported-period-values).

Setting a short period allows you to trade on shorter trends, but be aware that these will be noisier than longer periods.

**Values:** numerical– represents candlestick size in minutes.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### MACD Short <a id="macd-short"></a>

The number of periods used for calculating the short-term moving average for MACD.

**Values:** numerical, represents a number of periods.

**Default value:** 5

Parameter name in `config.js`: `MACD_SHORT`

### MACD Long <a id="macd-long"></a>

The number of periods used for calculating the long-term moving average for MACD.

**Values:** numerical, represents a number of periods.

**Default value:** 20

Parameter name in `config.js`: `MACD_LONG`

### MACD Signal <a id="macd-signal"></a>

The number of periods used for calculating the MACD signal line.

**Values:** numerical, represents a number of periods.

**Default value:** 10

Parameter name in `config.js`: `MACD_SIGNAL`

## Balance settings <a id="balance-settings"></a>

## **Confirming indicators + advanced indicator settings** <a id="confirming-indicators-advanced-indicator-settings"></a>

## **Misc settings** <a id="misc-settings"></a>

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

DCA is not intented to be used for margin trading.

## Reversal trading settings <a id="reversal-trading-settings"></a>

RT is not intented to be used for margin trading.

## TrailMe settings <a id="trailme-settings"></a>

TrailMe is not intended to be used with this strategy.

## Placeholders <a id="placeholders"></a>

The following parameters in `config.js` have no function for this strategy and act as placeholder.

