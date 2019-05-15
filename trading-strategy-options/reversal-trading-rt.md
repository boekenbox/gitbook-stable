# Reversal trading \(RT\)

Reversal trading \(RT\) is a Gunbot technique to keep on trading profitably when prices move downwards after an asset has been purchased.

The principle is to use the already invested amount of base currency to accumulate more units of the quote currency after prices have moved downwards. When prices keep going down, or go sideways at a lower level than the initial purchase, RT can keep accumulating until prices move up enough to sell the accumulated amount for an overall profit.

Trading fees paid while in reversal trading are all accounted for.

![](https://user-images.githubusercontent.com/2372008/38331646-13c489aa-3854-11e8-95b4-8b9143edf69b.PNG)

_Note that this example is kept simple intentionally. Prices don't have to go straight down for RT to successfully accumulate._

## How it works

Reversal trading starts when the current price is a set percentage lower than the last bought price, this is defined with `RT_GAIN`. The initial bag gets sold for base currency \(RT\_SELL\) and the invested amount is reserved for buying back more units when prices drop further. When the price then drops by a percentage defined with `RT_BUY_LEVEL`, quote currency gets bought \(RT\_BUY\). You now own more quote currency than you initially bought, at a lower price per unit.

This process will repeat when prices keep dropping, enabling you to keep accumulating quote currency without investing additional assets. Required funds are locked for the pair in reversal trading, and can't be used by other pairs.

With `TM_RT_SELL`, or when using `bb` as selling strategy, it's possible to place an RT\_SELL at a higher rate than the previous RT\_BUY, enabling you to reach a profitable exit point much faster.

When prices reach an overall profitable price \(EXIT POINT\), a normal sell order is placed using the sell criteria of your strategy.

In case prices recover to the break even point before an RT\_BUY could be made, the initial bag will be bought back to continue normal trading \(RT\_BUYBACK\). Alternatively, you can set a custom level for buying back quote with `RT_BUY_UP_LEVEL`.

{% hint style="info" %}

**Tips**

* Do not activate reversal trading on existing bags that are already down a lot unless you use`TM_RT_SELL`! The decision to run reversal trading or not should best be made before you start trading a pair, this way the process can kick in timely.
* Reversal trading math is done based on your trading history, if your last sell order was at loss \(and not caused by stop limit\), reversal trading would immediately start when you enable it and continues until it manages to profitably end the RT cycle - even when you've disabled RT again. 
* To prevent unwanted reversal trading, make sure to either have a profitable last sell order or to have set `IGNORE_TRADES_BEFORE` at a time after your last sell order at loss. To be sure, delete the pairs state JSON file after setting `IGNORE_TRADES_BEFORE`. 
* You can set a maximum price difference between current price and average bought price with `RT_MAXBAG_PROTECTION`, to prevent RT from starting on pairs that already lost a lot of value.

## RT flowcharts

There are three different ways Gunbot handles reversal trading, based on main strategies used for a pair. The chosen buy strategy affects the way RT\_BUY orders are executed, the sell strategy affects RT\_SELL orders.

Optional steps in the flowcharts are only relevant when `TM_RT_SELL` and/or `RT_TREND_ENABLED` are enabled.

### 1. Simplified flow for RT

![](https://user-images.githubusercontent.com/2372008/47076714-a5016d80-d1ff-11e8-9c5e-5d12b2a7e3e6.PNG)

_This flowchart shows the basic steps for reversal trading, not considering additional options like trailing or strategy specific conditions._

### 2. RT process for all strategies except `bb`

![](https://user-images.githubusercontent.com/2372008/47029958-02001380-d16d-11e8-8f7d-9f146fbbf24b.PNG)

### 3. RT process for `bb`

![](https://user-images.githubusercontent.com/2372008/47029957-02001380-d16d-11e8-8cd6-f661deadf73d.PNG)

`LOW_BB`_/_`HIGH_BB` _in reversal trading use the same settings as with regular trading on_ `bb`_._

## Relevant settings

Following settings options are available for reversal trading.

### RT Enabled

{% tabs %}
{% tab title="Description" %}
When set to true and prices drop, reversal trading will try to use the assets originally invested in your bag to accumulate more units, which can be sold for profit earlier than the original bag.

When double up is enabled, RT will start when DU\_CAP\_COUNT is reached.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Strategy buy |
| RT buyback | Strategy sell |
| RT sell | Close |
|  | DCA buy |
|  | Stop limit |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_ENABLED`
{% endtab %}
{% endtabs %}

### RT Gain

{% tabs %}
{% tab title="Description" %}
Defines the percentage drop after initial buy or RT\_BUY to trigger an RT\_SELL. Make sure to set this higher then the spread between bid and ask to prevent unwanted buybacks.

When set to 2 and the last buy had a price of 100, an RT\_SELL occurs when price is 98 or lower. Reversal trading will then wait for prices to drop by `RT_BUY_LEVEL` and buy more units back.

When prices move upwards instead of downwards it can happen that the bag gets bought back at the break-even price.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 1.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_GAIN`
{% endtab %}
{% endtabs %}

### RT Buy Level

{% tabs %}
{% tab title="Description" %}
This defines the percentage the price has to drop after RT\_SELL to trigger RT\_BUY.

When set to 2 and the last RT\_SELL happened at a price of 100, an RT\_BUY occurs when price is 98 or lower. Reversal trading will then wait to sell for profit, or for another RT\_SELL when prices keep dropping.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_BUY_LEVEL`
{% endtab %}
{% endtabs %}

### RT Sell Up

{% tabs %}
{% tab title="Description" %}
This sets the starting point for trailing up an RT\_SELL. Only works when `TM_RT_SELL` is enabled.

When you set this to 1 and price increases 1% after an RT\_BUY, sell trailing gets activated to place the next RT\_SELL as high as possible. The sell range is configurable with `TRAIL_ME_RT_SELL_RANGE`.

Optionally, you can use `RT_TREND_ENABLED` to only proceed with RT\_SELL\_UP when forecast trend indicates a strong uptrend.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage above the last buy price.

**Default value:** 0.3
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_SELL_UP`
{% endtab %}
{% endtabs %}

### RT Buy Up Level

{% tabs %}
{% tab title="Description" %}
This sets the price to place an RT\_BUY order above the last RT\_SELL order, the price must be below the break-even point for this to work. The default value of 0 disables this feature.

When you set this to 3 and price increases 3% after an RT\_SELL, an RT\_BUY order will be placed there instead of waiting for the price to hit the buyback point. \(Technically, this type of order is a buyback order, not a regular RT\_BUY\).

Beware that will have a negative effect on the amount of quote units accumulated during RT, it functions as a kind of stop loss for reversal trading.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage above the last sell price.

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buyback | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT sell |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_BUY_UP_LEVEL`
{% endtab %}
{% endtabs %}

### RT Trend Enabled

{% tabs %}
{% tab title="Description" %}
Enables the use of trend forecast for placing RT\_BUY or RT\_SELL orders when using `TM_RT_SELL` and/or `TRAIL_ME_RT`.

The forecast trend indicator combines smacross, xtrend and the time series forecast to provide an indication of the strength of a trend. This can be used to only place RT\_BUY or RT\_SELL orders when there is respectively a strong down- or uptrend.

An RT\_SELL order will be placed when the trailing stop hits and forecast trend shows 6 green arrows. An RT\_BUY order will be placed when the trailing stop hits and forecast trend shows 6 red arrows.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Strategy buy |
| RT sell | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_TREND_ENABLED`
{% endtab %}
{% endtabs %}

### RT Once

{% tabs %}
{% tab title="Description" %}
Set this to true to only allow one full RT cycle \(until final strategy sell\), after that the pair is set to not cycle again.

At the end of the RT cycle, the pair `enabled` setting will be set to false.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_ONCE`
{% endtab %}
{% endtabs %}

### RT Once And Continue

{% tabs %}
{% tab title="Description" %}
Set this to true to only allow one full RT cycle \(until final strategy sell\), after that RT will be disabled to continue normal trading.

At the end of the RT cycle, the `RT_ENABLED` setting will be set to false for the pair.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_ONCE_AND_CONTINUE`
{% endtab %}
{% endtabs %}

### RT Maxbag Protection

{% tabs %}
{% tab title="Description" %}
Sets the maximum difference between the average bought price and current price for starting RT. When the difference is bigger, RT orders won't be placed.

This is used as a protection against starting reversal trading on bags that already dropped too much for the process to work effectively.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical - represents a percentage.

**Default value:** 10
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RT_MAXBAG_PROTECTION`
{% endtab %}
{% endtabs %}

Reversal trading depends on several TrailMe settings to reach better entry points for RT\_BUY and to make RT\_SELL\_UP work. The relevant settings are listed below.

### Trail Me RT

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for RT\_BUY orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_RT`
{% endtab %}
{% endtabs %}

### Trail Me RT Sell

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for RT\_SELL orders above the last RT\_BUY rate.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TM_RT_SELL`
{% endtab %}
{% endtabs %}

### Trail Me Buy Range

{% tabs %}
{% tab title="Description" %}
This sets the buy range for TrailMe.

Setting a range of 0.5% at a starting price of 0.1 would set a range between 0.0995 and 0.1005. As long as prices keep moving downwards, the range moves down along with the price.

As soon as prices start going upward, the range freezes and a buy order is placed when the price crosses the upper boundary of the range.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT sell |
| RT Buy | Strategy sell |
| DCA buy | Close |
|  | Stop limit |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_BUY_RANGE`
{% endtab %}
{% endtabs %}

### Trail Me RT Sell Range

{% tabs %}
{% tab title="Description" %}
This sets the sell range for TrailMe.

Setting a range of 0.5% at a current price of 0.1 would set a range between 0.0995 and 0.1005. As long as prices keep moving upwards, the range moves up along with the price.

As soon as prices start going downward, the range freezes and a sell order is placed when the prices crosses the lower boundary of the range.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_RT_SELL_RANGE`
{% endtab %}
{% endtabs %}

