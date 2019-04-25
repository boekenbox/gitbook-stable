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

Following settings options are available Double Up.

Reversal trading is available in most Gunbot strategies, on each strategy page you'll find an overview with only the relevant settings for that strategy.

Double up depends on several TrailMe settings to reach better entry points. The relevant settings are listed below.

