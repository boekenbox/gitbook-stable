---
description: How to connect Gunbot to your exchange account.
---

# Exchanges

To be able to trade, you need to enter the exchange [API key](../../about/supported-exchanges/creating-api-keys.md) and secret, as well as the trading fees level for each exchange.

To enter these, go to **Profiles &gt; Exchanges**.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/image-15.png)

{% hint style="success" %}
The master key is just the API key you've registered for an exchange.

Using the same registered key for both the "master key" and "key" fields is OK.
{% endhint %}

Select your exchange and fill in all the fields for this exchange.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Master Key</b>
      </td>
      <td style="text-align:left">
        <p>The API key registered to be used with Gunbot.</p>
        <p>
          <br />This is the key you&apos;ve registered during an order, or have entered
          on the &quot;swap exchanges&quot; page. Each exchange has it&apos;s own
          master key.
          <br />
        </p>
        <p>This key may have read only access as long as you use a different Key
          for actual trading.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Key</b>
      </td>
      <td style="text-align:left">
        <p>The API key used for trading, can be the same as Master Key.</p>
        <p>This key must exist in the same exchange account as the Master Key.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Master Secret</b>
      </td>
      <td style="text-align:left">API secret belonging to &quot;master key&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Secret</b>
      </td>
      <td style="text-align:left">API secret belonging to &quot;key&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Delay</b>
      </td>
      <td style="text-align:left">
        <p>Time in seconds between processing two pairs on the same exchange.</p>
        <p>10 is generally a safe value, you can try lower delays if you see the
          bot running without errors for several hours.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Trading Fees</b>
      </td>
      <td style="text-align:left">Fee percentage per trade, use the appropriate value for your exchange
        and fees level.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Passphrase</b> (needed for select exchanges)</td>
      <td style="text-align:left">Some exchanges require a passphrase for each API key, it may be called
        slightly different on the exchange itself.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Market</b> (needed for select exchanges)</td>
      <td style="text-align:left">Some exchanges require specifying the market the bot will be used on:
        spot, swap or futures. Applies to Okex and BitGet.</td>
    </tr>
  </tbody>
</table>

