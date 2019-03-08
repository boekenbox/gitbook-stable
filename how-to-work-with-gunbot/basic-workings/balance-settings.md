# Important settings

{% hint style="info" %}
Every trading strategy contains a few balance settings, these are among the most important settings and you must understand what they do. 
{% endhint %}

## Trading Limit

With this setting you tell Gunbot how much to spend on a buy order.

The trading limit is generally defined in the "base currency" of a pair. This means that if you want to buy LTC with BTC, the trading limit must be set in Bitcoin. If you want to buy BTC with USD, you need to set the trading limit as an amount in US dollars.

Since in crypto there are many different base currencies, it is very important you set this correctly. Not setting it correctly can lead to trades for unwanted amounts, or no trades at all.

It is important to set the trading limit at least a bit higher than the minimum trade size of your exchange. 

The reason for this is that due to rounding and/or fees a trade mostly results in an amount worth slightly less than the trading limit. If this amount is lower than the minimum trade size of your exchange, it cannot be sold without buying additional units.



## Min volume to sell

This setting defines the minimum trade size for the pair you trade, this is both pair and exchange specific. When set correctly, Gunbot will ignore low balances that can not be sold.

Look up the minimum trade size for your pair, and set it exactly like this.

**Usage example:** 

* **Trading pair:** USDT-BTC
* **Exchange defined minimum trade size:** 10 USDT
* **Current balance value:** 8 USDT

When min volume to sell is correctly set to 10, Gunbot will ignore the current balance and proceed with looking for a buy opportunity.

When min volume to buy is set incorrectly, for example to 3, Gunbot will attempt to sell the current balance worth only 8 USDT. This will fail because the exchange does not allow the trade.



## Min volume to buy 

This setting is very similar to min volume to sell, it defines what the minimum amount is that Gunbot can place a buy order for. 

Look up the minimum trade size for your pair, and set it exactly like this. It is normal to set this parameter exactly the same as min volume to buy.

In some cases Gunbot will place a buy order in multiple parts, the setting min volume to buy tells the bot what the absolute minimum amount for a partial order can be. If you set it too low, such trades will fail.









