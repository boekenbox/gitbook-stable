---
description: Average down automatically.
---

# Dollar Cost Avg \(DCA\)

Double Up \(DU\) is a Gunbot feature to automatically average down assets, bringing down the average cost per unit when prices drop after a regular buy order.

By bringing the average price per unit down, the price to exit the trade profitably gets lower. Of course, the obvious risk of averaging down is investing more in units that already dropped in price. Use this feature carefully.

{% hint style="info" %}
Use the [DU guestimator sheet](https://docs.google.com/spreadsheets/d/1fTqYl-KkqYo1bCTQm1CoS1PcSoWW-oBFf2iLfXYFKyk) to simulate your DU settings.

Feel free to make a copy of the file. Thanks [Trashdog01](https://telegram.me/Trashdog01)!
{% endhint %}

## How it works

Below is an example of using Double up. The relevant settings used in the example would be:

* `DU_METHOD` set to `HIGHBB`. With this method each DU buy is triggered when the upper Bollinger Band crosses down the last buy price.
* `DOUBLE_UP_CAP` set to 1. Each DU buy is for 1:1 the amount of quote units already owned.
* `DU_CAP_COUNT` set to 2. A maximum of two DU buys are allowed.
* `DU_BUYDOWN` set to 1. The minimum required price difference for a DU buy compared to the buy price of the previous order.

![](https://user-images.githubusercontent.com/2372008/43907676-55eda770-9bf6-11e8-8124-60fb0d8d893d.PNG)

_Note that this example is kept simple intentionally, the amounts do not reflect real trades._

Using the example above: this is an overview of the buy orders made - notice how the average price per unit moves down after each DU buy:

|  | **Units** | **Price per unit** | **Invested \(cumulative\)** | **Avg Price per Unit** |
| :--- | :--- | :--- | :--- | :--- |
| **Buy** | 100 | 12.500 | 1.250.000 | 12500 |
| **DU Buy 1** | 100 | 10.800 | 2.330.000 | 11650 |
| **DU Buy 2** | 200 | 8.090 | 3.948.000 | 9870 |

The resulting sell order would have profitably sold all available units at a much lower price than the initial buy order:

|  | **Units** | **Price per unit** | **Proceeds** |
| :--- | :--- | :--- | :--- |
| **Sell** | 400 | 10.000 | 4.000.000 |

Configurable options for averaging down are:

* **Buydown:** A minimum price difference between the last buy and next DU buy. Regardless which DU method is picked, buydown must be reached for a DU buy.
* **DU method:** **HIGHBB** as shown above, **RSI** where DU buys are only placed within a configurable RSI range, and a **numerical** value defining a percentage price drop compared to the last bought price.
* **Ratio:** Defines how much extra quote units are bought for each DU buy.
* **Frequency:** How many DU buy orders are allowed.
* **Trailing:** DU buy orders can optionally use trailing.

{% hint style="danger" %}
Double Up can invest large amounts of base currency.

Be careful about the set ratio and frequency, make sure you have enough free funds.
{% endhint %}

## Relevant settings

Following settings options are available Double Up. None of them are relevant in margin strategies.

### Double Up

{% tabs %}
{% tab title="Description" %}
When set to true, `DOUBLE_UP` will try to get rid of bags by averaging down. Works on all strategies. Averaging down can use up a lot of balance, make sure you have enough of your base currency available to cover each double up buy. Gunbot will start averaging down a bag according to your setting for `DU_METHOD`.

Averaging down means that you buy more units at a lower price, bringing down the average price per unit.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DOUBLE_UP`
{% endtab %}
{% endtabs %}

### DU Method

{% tabs %}
{% tab title="Description" %}
This sets the trigger for placing buy orders with double up.

When set to **HIGHBB** Gunbot will start averaging down a bag when the actual upper Bollinger Band drops below bought price \(not the distance from it as set in `HIGH_BB`\) and the current price is a percentage below last bought price, as set in `DU_BUYDOWN`.

When set to **RSI**, buy orders will only be placed when the set `RSI_BUY_LEVEL` is reached and the current price is a percentage below last bought price, as set in `DU_BUYDOWN`.

When set to a **numerical** value like 2, buy orders will be placed when the price drops by 2% compared to the last bought price. Keep in mind that `DU_BUYDOWN` must still be reached when using numerical values for `DU_METHOD`.
{% endtab %}

{% tab title="Values" %}
**Values:** HIGHBB, RSI, or a numerical value representing a percentage

**Default value:** HIGHBB
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DU_METHOD`
{% endtab %}
{% endtabs %}

### Double Up Cap

{% tabs %}
{% tab title="Description" %}
This defines the ratio to the pairs balance to be used for each consecutive buy when doubling up. Setting it to 0.5 would mean it uses a 0.5:1 ratio for averaging down. 

It is recommended to set this as high as you can afford, to increase your chance to actually average down and sell at profit. Make sure that the resulting amount for the first double up order exceeds `MIN_VOLUME_TO_SELL`

Example with ratio of 1: initial buy of 100 LTC, first double up buy order is 100 LTC, second will be 200 LTC, then 400 LTC, etc. Example with 0.5 ratio: initial buy of 100 LTC, first double up buy order is 50 LTC, then 75 LTC, then 112.5 LTC.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a ratio.

**Default value:** 1
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DOUBLE_UP_CAP`
{% endtab %}
{% endtabs %}

### DU Cap Count

{% tabs %}
{% tab title="Description" %}
Sets the maximum number of times double up orders may be placed.

When set to 1, a maximum number of 1 double up orders will be placed to average down. When you set this higher, double up can place more orders and has a higher chance of effectively averaging down and come closer to a profitable exit point each time it places a double up order.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical

**Default value:** 0
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DU_CAP_COUNT`
{% endtab %}
{% endtabs %}

### DU Buydown

{% tabs %}
{% tab title="Description" %}
The minimum price drop compared to the last bought price that needs to occur for double up buys to be placed.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage

**Default value:** 2
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `DU_BUYDOWN`
{% endtab %}
{% endtabs %}

### RSI DU Buy

{% tabs %}
{% tab title="Description" %}
 Use this to specify the maximum RSI level for buying when `DU_METHOD` is set to RSI.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, ranging between 1 and 99

**Default value:** 30
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RSI_DU_BUY`
{% endtab %}
{% endtabs %}

Double up depends on several TrailMe settings to reach better entry points. The relevant settings are listed below.

### Trail Me DU

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for double up orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | RT sell |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_DU`
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

### 

