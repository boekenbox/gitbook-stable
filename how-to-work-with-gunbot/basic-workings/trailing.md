---
description: How to work with price trailing for various order types.
---

# Trailing

Trailing is an important concept in Gunbot. When buying, it allows for aiming for a lower entry price than strategy criteria would otherwise realize. When selling, it allows for higher profits, while locking in profits while trailing ranges move upwards along with price.

Many different order types can use trailing, the basic principles are always the same:

* Trailing is purely based on prices, not on indicators. 
* When confirming indicators are used, trailing must finish while their conditions are still met.
* When buying, the ask price is used as reference. When selling, bid is used.
* Ranges for trailing are defined as a percentage of the best bid or ask price. Bid is used when selling, ask is used when buying.
* When price moves in the direction of your target, ranges are set around bid/ask every round.
* When price moves in the opposite direction of your target, trailing limits are frozen.
* Ranges are reset when price moves outside of the target.

## Buy trailing

The same basic process applies for most kinds of buy trailing \(exception: Take Buy\). The example below shows how trailing works for buying with the [tssl strategy](../../trading-strategy-options/regular-strategies-spot-trading/tssl-trailing-stop-stop-limit.md).

![](../../.gitbook/assets/image%20%2854%29.png)

When trailing with tssl, the most important part to know is that trailing must finish below Buy Level. Make sure to set a Buy Level that leaves enough room for trailing.

Other types of buy trailing, like DU trailing, follow the exact same trailing logic, but the threshold below which trailing must finish is different.

| Type of trailing | Must finish below / while |
| :--- | :--- |
| TrailMe Buy | While other strategy buy criteria are met |
| TrailMe DU | Below DU Buydown |
| TrailMe RT Buy | Below RT Buy Level |
| Tssl | Below Buy Level, while other strategy buy criteria are met |

## Sell trailing

The same basic process applies for most kinds of sell trailing \(exception: Take Profit\). The example below shows how trailing works for selling with the [tssl strategy](../../trading-strategy-options/regular-strategies-spot-trading/tssl-trailing-stop-stop-limit.md).

This example looks more complicated than the buy example below, because the concept of resetting trailing limits is now visualized.

![](../../.gitbook/assets/image%20%285%29.png)

Important to know about sell trailing with tssl is that the StopLoss limit must be above your setting for gain before trailing can finish. Make sure that you leave enough room for this to happen, either by setting a modest gain, or by using a small sell range.

Other types of sell trailing behave in the same way, but instead of gain a different reference point is used.

| Type of trailing | Must finish above / while |
| :--- | :--- |
| TrailMe Sell | While other strategy sell criteria are met |
| TrailMe RT Sell Up | Above RT Sell Up |
| Tssl | Above Gain, while other strategy sell criteria are met |

## Take Buy

Buy trailing with Take Buy is a bit different, because it acts as a second buy layer in your strategy.

The idea is that sometimes prices will come close to your target, but not close enough for your main strategy to catch a buy. In this case you can use Take Buy as an extra threshold above your regular entry point.

![](../../.gitbook/assets/image%20%2847%29.png)

The example above shows that price was on its way reaching the primary buy trigger Buy Level, but didn't quite make it. Instead, Take Buy took the order when prices started moving upwards again.

## Take Profit

Similar to Take Buy, Take Profit acts as a secondary sell layer in your strategy. It trails between the break-even point and the target for Gain. In case prices do move over your Gain target, trailing stops and the normal strategy conditions apply.

![](../../.gitbook/assets/image%20%2860%29.png)

