---
description: 'Adding trading pairs to Gunbot, information about how cycling works.'
---

# Trading pairs

To configure which trading pairs Gunbot should trade, go to **Pairs** or **Easy edit**.

You can use an unlimited number of trading pairs, across multiple exchanges.

{% page-ref page="pair-processing.md" %}

## Add trading pairs

![](../../../.gitbook/assets/image%20%28115%29.png)

To start trading on a new pair, just enter or select the pair name, pick the exchange and strategy and hit the **Add** button. When you want to temporarily stop trading a pair, use the **Enabled** toggle to disable the pair.

On exchanges that support it, a pair list is automatically provided in the correct format. For some exchanges you need add pair names manually.

{% hint style="success" %}
The _Use safe balance settings_ option makes sure that a few critical balance settings are automatically set to known working values, for each newly added BTC, USD, USDT or EUR pair.
{% endhint %}

### Using filters to add multiple pairs at once

Select the **Use filter rules** option to add pairs based on filters you set. This way you can for example easily add the top 10 volume USDT pairs.

When you're done, hit the **Filter** button to add pairs matching your filters.

![](../../../.gitbook/assets/image%20%28117%29.png)

{% hint style="warning" %}
Some exchange unfortunately just don't provide the data needed to filter pairs in this way, or their API rate limits are just too strict to do it in a practical way. In that case likely just nothing happens when you hit the filter button.
{% endhint %}

## Pair naming conventions

Gunbot uses a standardized format for entering trading pairs, this allows you to use the same syntax for all exchanges you might use.

Where possible it uses this notation: BASECOIN-QUOTECOIN

All pairs with BTC as base currency are written like: BTC-ETH, BTC-ATOM, BTC-ETH

All pairs with USDT as base currency are written like: USDT-BTC, USDT-ETH, USDT-XMR

The base coin is the one used to buy another asset. Be aware that some exchange show pair names in the exact reverse order. _\*\*_

## Symbol name exceptions

A small number of symbol names need to be written in a slightly different way than the exchange lists them on their site.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Exchange</th>
      <th style="text-align:left">Symbol exceptions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">binance</td>
      <td style="text-align:left">Use YOYOW instead of YOYO</td>
    </tr>
    <tr>
      <td style="text-align:left">bitfinex</td>
      <td style="text-align:left">
        <p>For a few symbols, the API display name is required. These are:</p>
        <p>IOTA = IOT</p>
        <p>DASH = DSH</p>
        <p>QTUM = QTM</p>
        <p>DATA = DAT</p>
        <p>QASH = QSH</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">kraken</td>
      <td style="text-align:left">Use BTC instead of XBT</td>
    </tr>
  </tbody>
</table>

## Pair naming for futures exchanges

On **Binance Futures** USDT margined pairs, naming works the same as for spot trading: USDT-BTC, USDT-LTC, etc. For coin margined pairs the correct format is: USD-BTC-PERP or \_\*\*\_USD-COIN-EXPIRYDATE. It's recommended to use the GUI to just select the right pair names. Set `market: futures` to use USDT-margined pairs, set `market: delivery` to use coin margined pairs.

Pairs on **Bitmex** use almost the same symbols as on Bitmex itself, but with a hyphen-minus between the two asset names. Example: XBT-USD

Pairs on **Kraken Futures** follow the following conventions:

* Perpetual contracts: XBT-USD, ETH-USD, LTC-USD, etc.
* Monthly futures: XBT-MONTH, ETH-MONTH, LTC-MONTH, etc
* Quarterly futures: XBT-QUART, ETH-QUART, LTC-QUART, etc

Pairs on **Okex Futures** use the similar notation as on their site, with hyphens in between the different parts of the pair and swapping the symbols. Valid example: USD-BTC-201016

## Override settings

Overrides are pair specific settings, overruling the assigned strategy. Every strategy parameter can be used as an override.

![](../../../.gitbook/assets/image%20%28105%29.png)

You can use this, for example, to set a different trading limit for a specific pair.

When adding overrides, you can choose from a list of all available strategy settings. See the strategy settings pages for which values are accepted for each individual parameter.

{% hint style="info" %}
Make sure to only add overrides for settings that actually have a function for the buy and sell methods of your strategy. See the strategy pages for detailed info about relevant settings.
{% endhint %}

