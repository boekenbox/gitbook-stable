# Confirming indicators

To further restrict trading criteria for your strategy, you can use a number of confirming indicators for regular buy and sell orders. This page describes all settings options available for confirming indicators.

Orders are placed when both the main strategy settings and confirming indicators meet the market situation. When TrailMe is used too, it must finish trailing while other conditions meet. All these conditions must occur in the same cycle.

Especially when TrailMe is used together with confirming indicators, it makes sense to not set the indicators too strict as all order criteria must happen in the same cycle for an order to be placed \(strategy conditions + indicator conditions + hitting trailing stop\).

{% hint style="success" %}
**Indicators in Gunbot are calculated with live data.**

For example for a 14 period RSI calculation, that means that the period close values for the past 13 completed candles are used, plus the live data for the current cycle.
{% endhint %}

{% hint style="info" %}
For margin trading, the buy side for indicator settings apply for opening long position. The sell side applies for opening short positions.
{% endhint %}

## ADX

### ADX Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will enable ADX as a confirming indicator, only allowing trades when the trend is strong enough to meet or exceeds the set ADX\_LEVEL.

ADX measures both up- and downtrends, when DI+ is lower than DI- prices are moving up \(these values are visible in the logs\). When DI- is lower than DI+, prices are moving down. The ADX value indicates the strength of the current up- or downtrend.

A buy order is confirmed when ADX is above ADX\_LEVEL and DI- is lower than DI+.

A sell order is confirmed when ADX is above ADX\_LEVEL and DI- is higher than DI+.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
Parameter name in `config.js`: `ADX_ENABLED`
{% endtab %}
{% endtabs %}

### ADX Level

{% tabs %}
{% tab title="Description" %}
Sets the minimum trend level that needs to be reached for orders to be allowed.

When set to 25, trades will be placed as soon as ADX is 25 or higher.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 25
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
Parameter name in `config.js`: `ADX_LEVEL`
{% endtab %}
{% endtabs %}

### DI Period

{% tabs %}
{% tab title="Description" %}
Sets the number of candles used to calculate ADX.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 14
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
Parameter name in `config.js`: `DI_PERIOD`
{% endtab %}
{% endtabs %}

## BTC PND protection

### BTC PND Protection

{% tabs %}
{% tab title="Description" %}
Setting this to true disables buy orders when there is too much price and volume pressure on BTC.

This setting is dependent on `BTC_MONEY_FLOW`. Only use this if BTC pumps have a significant effect on your trading pair.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
Parameter name in `config.js`: `BTC_PND_PROTECTION`
{% endtab %}
{% endtabs %}

### BTC Money Flow

{% tabs %}
{% tab title="Description" %}
Sets the value on the Money Flow Index \(MFI\) for BTC-USD that `BTC_PND_PROTECTION` disables orders for. As soon as MFI hits the set value or drops below it, `BTC_PND_PROTECTION` will be active.

The default value of 35 indicates that the BTC-USD market is moving into oversold territory and might start pumping soon, no buy orders would be placed while BTC-USD MFI is between 35 and 0.

As long as BTC-USD is the defined oversold territory, no orders will be placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 25
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
Parameter name in `config.js`: `BTC_MONEY_FLOW`
{% endtab %}
{% endtabs %}

### BTC PND Period

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating MFI for BTC\_PND\_PROTECTION.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 14
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
Parameter name in `config.js`: `BTC_PND_PERIOD`
{% endtab %}
{% endtabs %}

## EMA Spread

### EMA Spread

{% tabs %}
{% tab title="Description" %}
Setting this to true will enable EMA spread as a confirming indicator for both buy and sell orders. The spread is calculated each cycle by subtracting the lowest EMA value from the highest EMA.

A buy signal occurs when EMA1 \(slow EMA\) is at least `EMAx` higher than EMA2 \(fast EMA\) and the EMA spread value starts to decrease \(after having increased first\).

A sell signal occurs when EMA1 \(slow EMA\) is at least EMAx lower than EMA2 \(fast EMA\) and the EMA spread value starts to increase \(after having decreased first\).
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
Parameter name in `config.js`: `EMASPREAD`
{% endtab %}
{% endtabs %}

### EMAx

{% tabs %}
{% tab title="Description" %}
Sets the minimum percentage difference between slow and fast EMA for `EMASPREAD`.

When set to 1, the spread must reach at least 1% before `EMASPREAD` can trigger a buy or sell.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

**Default value:** 0.5
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
Parameter name in `config.js`: `EMAx`
{% endtab %}
{% endtabs %}

## MFI

### MFI Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will make sure Gunbot only trades when both strategy conditions and `MFI_BUY_LEVEL` / `MFI_SELL_LEVEL` are met.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
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
Parameter name in `config.js`: `MFI_ENABLED`
{% endtab %}
{% endtabs %}

### MFI Buy Level

{% tabs %}
{% tab title="Description" %}
Defines the maximum MFI level you want to allow buy orders at.

When set to 30, buy orders will only be placed when MFI is between 0 and 30.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 30
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MFI_BUY_LEVEL`
{% endtab %}
{% endtabs %}

### MFI Sell Level

{% tabs %}
{% tab title="Description" %}
Set this to the minimum MFI level you want to allow sell orders at.

For example: when set to 70, sell orders will only be placed when MFI is between 70 and 100.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 70
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `MFI_SELL_LEVEL`
{% endtab %}
{% endtabs %}

### MFI Length

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating MFI.

MFI is calculated using an array of the period close prices of MFI\_LENGTH-1 candles and the last price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 14
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
Parameter name in `config.js`: `MFI_LENGTH`
{% endtab %}
{% endtabs %}

## RSI

### RSI Buy Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will make sure Gunbot only buys when both strategy buying conditions and `RSI_BUY_LEVEL` are met.
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
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_BUY_ENABLED`
{% endtab %}
{% endtabs %}

### RSI Sell Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will make sure Gunbot only sells when both strategy selling conditions and `RSI_SELL_LEVEL` are met.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_SELL_ENABLED`
{% endtab %}
{% endtabs %}

### RSI Method

{% tabs %}
{% tab title="Description" %}
Sets the method for using RSI. See `RSI_BUY_LEVEL` and `RSI_SELL_LEVEL` for detailed descriptions of both methods.
{% endtab %}

{% tab title="Values" %}
**Values:** oscillator or cross.

**Default value:** oscillator
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
Parameter name in `config.js`: `RSI_METHOD`
{% endtab %}
{% endtabs %}

### RSI Buy Level

{% tabs %}
{% tab title="Description" %}
Set this to the RSI level you want to allow buy orders at.

`RSI_METHOD` = oscillator: when set to 40, buy orders will only be placed when RSI is 40 or lower.

`RSI_METHOD` = cross: when set to 40, buy orders will only be placed when RSI crosses over 40.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 30
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_BUY_LEVEL`
{% endtab %}
{% endtabs %}

### RSI Sell Level

{% tabs %}
{% tab title="Description" %}
Set this to the RSI level you want to allow sell orders at.

`RSI_METHOD` = oscillator: when set to 60, sell orders will only be placed when RSI is 60 or higher.

`RSI_METHOD` = cross: when set to 60, sell orders will only be placed when RSI crosses under 60.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 70
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_SELL_LEVEL`
{% endtab %}
{% endtabs %}

### RSI Length

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating RSI.

RSI is calculated using an array of the period close prices of RSI\_LENGTH-1 candles and the last price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of candlestick periods.

**Default value:** 14
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| DCA buy \(when using RSI\) | RT sell |
|  | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_LENGTH`
{% endtab %}
{% endtabs %}

## Stochastic

### Stoch Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will make sure Gunbot only trades when both strategy trade conditions and `STOCH_BUY_LEVEL` / `STOCH_SELL_LEVEL` are met.

If you only want to use this indicator for buying or selling, but not both, then set the side you don't want to use to -1001.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
| Strategy sell | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCH_ENABLED`
{% endtab %}
{% endtabs %}

### Stoch Method

{% tabs %}
{% tab title="Description" %}
Sets the method for using Stochastic. See `STOCH_BUY_LEVEL` and `STOCH_SELL_LEVEL` for detailed describtions of both methods.
{% endtab %}

{% tab title="Values" %}
**Values:** oscillator or cross.

**Default value:** oscillator
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
Parameter name in `config.js`: `STOCH_METHOD`
{% endtab %}
{% endtabs %}

### Stoch Buy Level

{% tabs %}
{% tab title="Description" %}
Set this to the maximum Stochastic level you want to allow buy orders at.

When set to 30, buy orders will only be placed when Stochastic is between 0 and 30.

`STOCH_METHOD` = oscillator: a buy signal occurs when both Stoch %K and %D are below the set buy level.

`STOCH_METHOD` = cross: a buy signal occurs when both Stoch %K and %D are below the set buy level, additionally %K must cross over %D.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 30
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCH_BUY_LEVEL`
{% endtab %}
{% endtabs %}

### Stoch Sell Level

{% tabs %}
{% tab title="Description" %}
Set this to the minimum Stochastic level you want to allow sell orders at.

When set to 70, sell orders will only be placed when Stochastic is between 70 and 100.

`STOCH_METHOD` = oscillator: a sell signal occurs when both Stoch %K and %D are above the set sell level.

`STOCH_METHOD` = cross: a sell signal occurs when both Stoch %K and %D are above the set sell level, additionally %K must cross down %D.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99.

**Default value:** 70
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCH_SELL_LEVEL`
{% endtab %}
{% endtabs %}

### Stoch K

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating Stochastic %K.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 14
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
Parameter name in `config.js`: `STOCH_K`
{% endtab %}
{% endtabs %}

### Slow Stoch K

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating Slow Stochastic %K.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 3
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
Parameter name in `config.js`: `SLOW_STOCH_K`
{% endtab %}
{% endtabs %}

### Stoch D

{% tabs %}
{% tab title="Description" %}
The number of periods used for calculating Stochastic %D.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 3
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
Parameter name in `config.js`: `STOCH_D`
{% endtab %}
{% endtabs %}

## StochRSI

### Stoch RSI Enabled

{% tabs %}
{% tab title="Description" %}
Setting this to true will make sure Gunbot only trades when both strategy trade conditions and `STOCHRSI_BUY_LEVEL` / `STOCHRSI_SELL_LEVEL` are met.

If you only want to use this indicator for buying or selling, but not both, then set the side you don't want to use to -1001.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
| Strategy sell | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCHRSI_ENABLED`
{% endtab %}
{% endtabs %}

### Stoch RSI Method

{% tabs %}
{% tab title="Description" %}
Sets the method for using StochRSI. See `STOCHRSI_BUY_LEVEL` and `STOCHRSI_SELL_LEVEL` for detailed describtions of both methods.
{% endtab %}

{% tab title="Values" %}
**Values:** oscillator or cross.

**Default value:** oscillator
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
Parameter name in `config.js`: `STOCHRSI_METHOD`
{% endtab %}
{% endtabs %}

### Stoch RSI Buy Level

{% tabs %}
{% tab title="Description" %}
Set this to the StochRSI level you want to allow buy orders at.

`STOCHRSI_METHOD` = oscillator: when set to 0.2, buy orders will only be placed when StochRSI is 0.2 or lower.

`STOCHRSI_METHOD` = cross: when set to 0.2, buy orders will only be placed when StochRSI crosses over 0.2.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 0.01 and 0.99

**Default value:** 0.2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCHRSI_BUY_LEVEL`
{% endtab %}
{% endtabs %}

### Stoch RSI Sell Level

{% tabs %}
{% tab title="Description" %}
Set this to the StochRSI level you want to allow sell orders at.

STOCHRSI\_METHOD = oscillator: when set to 0.8, sell orders will only be placed when StochRSI is 0.8 or higher.

STOCHRSI\_METHOD = cross: when set to 0.8, sell orders will only be placed when StochRSI crosses under 0.8.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 0.01 and 0.99

**Default value:** 0.8
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | RT buyback |
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy |
|  | Strategy buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `STOCHRSI_SELL_LEVEL`
{% endtab %}
{% endtabs %}

### Stoch RSI Length

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want to use for calculating StochRSI.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 14
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
Parameter name in `config.js`: `STOCHRSI_LENGTH`
{% endtab %}
{% endtabs %}

## Advanced settings

### EMA Length

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want be available for calculating `EMA1` and `EMA2`. Unless you use very high EMA values, you usually do not need to change this.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 50
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
Parameter name in `config.js`: `EMALENGTH`
{% endtab %}
{% endtabs %}

### Candles Length

{% tabs %}
{% tab title="Description" %}
Set this to the number of candlestick periods you want Gunbot to pull from the exchange, which are available for calculating other indicators.

Always make sure that this setting is high enough for other indicators to be properly calculated.

Please be aware that exchanges won't always provide the number of specified candles, often it is less.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a number of periods.

**Default value:** 99
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| DCA buy \(when using an indicator as trigger\) | RT sell |
|  | Close |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `CANDLES_LENGTH`
{% endtab %}
{% endtabs %}

## Bollinger Bands for DCA

### SMA Period

{% tabs %}
{% tab title="Description" %}
This defines the number of periods used for calculating Bollinger Bands. Only used when `DU_METHOD` is set to HIGHBB.
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
This value defines the multiplier used for calculating Bollinger Bands. Only used when `DU_METHOD` is set to HIGHBB.
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

## Renko candles

![](https://user-images.githubusercontent.com/2372008/51115276-13a69500-1808-11e9-85b9-b221ef2cddeb.png)

[Renko](https://github.com/boekenbox/gitbook/tree/74bed25d66f4b0422a81f67f250bcb09c6cf1780/wiki/Renko_Charts/README.md) candles can be used with the ichimoku-margin strategy.

### Use Renko

{% tabs %}
{% tab title="Description" %}
Setting this to true will enable the use of renko candles, instead of regular candles.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `USE_RENKO`
{% endtab %}
{% endtabs %}

### Renko Period

{% tabs %}
{% tab title="Description" %}
Sets which regular candle size is used as input for renko candles. Make sure to set `PERIOD` to the same value as `RENKO_PERIOD`.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 15
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RENKO_PERIOD`
{% endtab %}
{% endtabs %}

### Renko Brick Size

{% tabs %}
{% tab title="Description" %}
Defines the brick size for each candle.

For example, when set to 1 on a pair with USD as base currency, each renko candle will represent a price change of at least 1 USD.

If you are unfamiliar with renko charts, it's recommended to first set the brick size to the smallest possible step, for example 1 USD or 0.00000001 BTC, and see what effect it has on the chart \(do keep the chart open a while to check how new price movements get visualized\). Then keep increasing the brick size until you get a good feeling of how the chart behaves.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 0.0001
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RENKO_BRICK_SIZE`
{% endtab %}
{% endtabs %}

### Renko ATR

{% tabs %}
{% tab title="Description" %}
Enable this to dynamically adjust brick size based on the average true range \(ATR\).

For fiat pairs, 1 pip equals 1 cent. For crypto, 1 pip equals 1 satoshi.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
| Strategy buy | RT buyback |
| Close | RT sell |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RENKO_ATR`
{% endtab %}
{% endtabs %}

