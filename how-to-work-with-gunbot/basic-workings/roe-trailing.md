---
description: How to work with ROE trailing for margin trading.
---

# ROE trailing

To increase the profitability of a margin position, you can use ROE trailing to trail for a better close price.

The basic concepts are exactly the same as regular buy and sell [trailing](trailing.md), with three differences:

* Trailing ranges are based on a percentage of the unrealized ROE for the position. This means that the trailing limits get wider when unrealized ROE increases.
* Instead of trailing prices, changes in unrealized ROE are trailed.
* The only dependency for ROE trailing is that it starts when the minimum ROE target is reached. Other factors like confirming indicators play no role.

![](../../.gitbook/assets/image%20%2823%29.png)

{% hint style="info" %}
Because trailing is completely based on ROE, and not on price, you must use larger trailing limits than with other types of trailing.

Setting a ROE Limit of 1 means that when your unrealized ROE decreases with 1% \(i.e. from 1.00 to 0.99\), the position will be closed. A low setting like this hardly allows for trailing.
{% endhint %}

