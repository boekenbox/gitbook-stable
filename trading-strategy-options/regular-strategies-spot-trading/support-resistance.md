# Support / Resistance

Buy at support, sell at resistance. That's all this strategy does. You can configure a distance from support and resistance, to configure a range where it will trade.

## How to work with this strategy

There is just a single setting that defines the entry point for this strategy: `SupRes_SPREAD`

This setting represents a percentage \(of price\) above the first support level \(when buying\), or below the first resistance level \(when selling\). As soon as price crosses this threshold, it will place an order. If the balance settings allow for multiple orders, a buy order gets placed every time the buy conditions are met.

In the example below, `SupRes_SPREAD` is set to 0.1, the "buy at" line visualizes the target. The sell target would be 0.1 % below the first resistance level.

![](../../.gitbook/assets/image%20%2875%29.png)

Keep in mind that support and resistance are not static targets. This makes the `SupRes_SPREAD` setting more or less a trailing range. It's very important to set a value that makes sense for the current pair and the price range it is in: too big of a spread can cause immediate trades.

The strategy sells when price crosses `SupRes_SPREAD` and `GAIN` is reached.

This strategy can buy multiple times, it can be capped with `SupRes_MAX`.

### Formula

Gunbot uses the following formula to calculate support and resistance levels. The number of candles uses as input is user configurable with the `SMAPERIOD`setting.

```text
P = (H + L + C) / 3 
R1 = (P  2) - L 
R2 = P + (H - L) 
S1 = (P  2) - H 
S2 = P - (H - L)
```

{% hint style="success" %}
**Less options than usual**

This strategy is a bit different from other strategies, it has much less configurable options. Confirming indicators or additional trailing are disabled.
{% endhint %}

## Strategy parameters

Following settings options are available for `SupportResistance` and can be set in the strategy configurator of the GUI or the strategies section of the config.js file.

These settings are global and apply to all pairs running this strategy. When you want a specific parameter to be different for one or more pairs, use an override at the pair level.

Using the `BUY_METHOD` and `SELL_METHOD` parameters you can combine different methods for buying and selling. This strategy page assumes both `BUY_METHOD` and `SELL_METHOD` are set to `SupportResistance`. Accepted values are all strategy names as listed [here](../about-gunbot-strategies/trading-methods.md#available-buy-and-sell-methods).

## Buy & sell settings

Buy settings are the primary trigger for buy orders. Sell settings control how sell orders are placed. These parameters control the execution of buy orders when using `SupportResistance` as buy & sell method.

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
This sets the minimum target for selling. Gunbot will sell once price reaches the set percentage above the break-even point. and `SupRes_SPREAD` is reached.

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

### Sup / Res spread

{% tabs %}
{% tab title="Description" %}
Sets the distance from support/resistance levels, in which orders may be placed.

For buying: this value is a percentage above the first support level.

For selling: this value is a percentage below the first resistance level.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage \(of price\)

**Default value:** 0.1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Stop limit |
| Strategy buy | RT sell |
|  | RT buy |
|  | RT buyback |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SupRes_SPREAD`
{% endtab %}
{% endtabs %}

### Sup / Res max

{% tabs %}
{% tab title="Description" %}
Use this setting to limit how many times the strategy is allowed to buy, by setting a maximum allowed position size in base currency.

If the combined value of quote balance, open orders and the next order to place are higher than `SupRes_MAX`, then no further buy orders will be placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a value in base currency

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Stop limit |
| Strategy buy | RT sell |
|  | RT buy |
|  | RT buyback |
|  | Close |
|  | DCA buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SupRes_MAX`
{% endtab %}
{% endtabs %}

## Indicator settings

These indicator settings have a direct effect on trading with `SupportResistance`.

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

### SMA Period

{% tabs %}
{% tab title="Description" %}
This defines the number of candles used for calculating support and resistance level.
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
|  | RT sell |
|  | Close |
|  | Stop limit |
|  | DCA buy order |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `SMAPERIOD`
{% endtab %}
{% endtabs %}

## Balance settings

{% page-ref page="../balance-settings.md" %}

## Misc settings

{% page-ref page="../misc-settings.md" %}

## DCA settings

This strategy cannot use DCA.  
Averaging down is natively implemented by allowing multiple buy orders.

## Reversal trading

This strategy cannot use reversal trading.

## TrailMe settings

This strategy cannot use TrailMe.

## Confirming indicators

This strategy cannot use confirming indicators.

