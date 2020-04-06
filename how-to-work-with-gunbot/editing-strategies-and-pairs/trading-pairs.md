---
description: 'Adding trading pairs to Gunbot, information about how cycling works.'
---

# Trading pairs

To configure which trading pairs Gunbot should trade, go to **Settings &gt; Trading &gt; Trading Pairs**.

You can use an unlimited number of trading pairs, across multiple exchanges.

**Pair processing example for 1 exchange:**

* 2 enabled trading pairs
* Exchange delay set to 5
* "Processing" below means that Gunbot retrieves data and trades when strategy trading conditions happen in that round
* First round of pair 1 is processed
* Waits 5 seconds \(as defined in the exchange delay\)
* First round of pair 2 is processed
* Waits 5 seconds
* Second round of pair 1 is processed
* Waits 5 seconds
* Second round of pair 2 is processed.
* ... continues cycling through further rounds

{% hint style="info" %}
**Parallel processing of multiple exchanges**

If you have trading pairs across multiple exchanges, each exchange will process pairs in parallel. This means that a pair on exchange 1 does not wait for pairs on exchange 2 to be processed first.

Every exchange will individually cycle through enabled pairs like described above. Each exchange can have it's individual delay setting.
{% endhint %}

## Add trading pairs

![](../../.gitbook/assets/image%20%2810%29.png)

Gunbot uses a standardized format for entering trading pairs, this allows you to use the same syntax for all exchanges you might use.

The format is: **BASECOIN-QUOTECOIN**, where the base coin is the one used to buy another asset. Be aware that some exchange show pair names in the exact reverse order.

To start trading on a new pair, just enter the pair name, pick the exchange and strategy and hit the **Add** button. When you want to temporarily stop trading a pair, use the **Enabled** toggle to disable the pair.

### Pair naming conventions

Gunbot normalizes pair notation, where possible it uses this notation: BASECOIN-QUOTECOIN

All pairs with BTC as base currency are written like: BTC-ETH, BTC-OK, BTC-XLM

All pairs with USDT as base currency are written like: USDT-BTC, USDT-ETH, USDT-XMR

### Exceptions

For a few coins on **Bitfinex**, the API display name is required. These are: IOTA = IOT, DASH = DSH, QTUM = QTM, DATA = DAT, QASH = QSH

**Kraken** calls Bitcoin XBT, use BTC instead.

Pairs on **Bitmex** use almost the same symbols as on Bitmex itself, but with a hyphen-minus between the two asset names. Example: XBT-USD

Pairs on **Kraken Futures** follow the following conventions:

* Perpetual contracts: XBT-USD, ETH-USD, LTC-USD, etc.
* Monthly futures: XBT-MONTH, ETH-MONTH, LTC-MONTH, etc
* Quarterly futures: XBT-QUART, ETH-QUART, LTC-QUART, etc

{% hint style="info" %}
**Pair naming conventions**

Gunbot normalizes pair notation, all pairs for all exchanges except Bitmex follow the same logic: BASECOIN-QUOTECOIN

All pairs with BTC as base currency are written like: BTC-ETH, BTC-OK, BTC-XLM

All pairs with USDT as base currency are written like: USDT-BTC, USDT-ETH, USDT-XMR

**Exceptions**

For a few coins on Bitfinex, the API display name is required. These are:

IOTA = IOT, DASH = DSH, QTUM = QTM, DATA = DAT, QASH = QSH

Kraken calls Bitcoin XBT, use BTC instead.

Pairs on Bitmex use almost the same symbols as on Bitmex itself, but with a hyphen-minus between the two asset names. Example: XBT-USD

Pairs on Kraken Futures follow the following conventions
{% endhint %}

## Override settings

Overrides are pair specific settings, overruling the assigned strategy. Every strategy parameter can be used as an override.

![](../../.gitbook/assets/image%20%2835%29.png)

You can use this, for example, to set a different `TRADING_LIMIT` for a specific pair.

When adding overrides, make sure to only enter accepted values as described on the strategy pages.

