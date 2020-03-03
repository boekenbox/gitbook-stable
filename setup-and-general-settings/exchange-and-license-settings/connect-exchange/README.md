---
description: How to connect Gunbot to your exchange account.
---

# Connect exchange

To be able to trade, you need to enter the exchange [API key](creating-api-keys.md) and secret, as well as the trading fees level for each exchange.

To enter these, go to **Settings &gt; Trading &gt; Exchanges**.

![](../../../.gitbook/assets/image%20%2825%29.png)

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
          <br /><b>This is the key you&apos;ve registered during an order, or have entered on the &quot;swap exchanges&quot; page. Each exchange has it&apos;s own master key.</b>
          <br
          />
        </p>
        <p>This key may have read only access as long as you use a different Key
          for actual trading.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Master Secret</b>
      </td>
      <td style="text-align:left">The API secret for the Master Key.</td>
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
      <td style="text-align:left"><b>Secret</b>
      </td>
      <td style="text-align:left">The API secret for the Key.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Delay</b>
      </td>
      <td style="text-align:left">
        <p>The delay factor (in seconds) for processing pairs on this exchange.</p>
        <p>Setting this to 10 should work in almost all cases, you can lower it later
          to speed up pair processing after you&apos;ve verified that everything
          works.</p>
      </td>
    </tr>
  </tbody>
</table>Some exchanges require extra settings like a passphrase. These are described below.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Field</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>clientId</b>
      </td>
      <td style="text-align:left">Your CEX client ID. Only relevant for CEX.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>passphrase</b>
      </td>
      <td style="text-align:left">
        <p>Your GDAX API passphrase. Only relevant for GDAX.</p>
        <p>
          <br />In case you use a different trading key than your master key, make sure
          that both keys use the same passphrase.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>passphrase</b>
      </td>
      <td style="text-align:left">
        <p>Your KuCoin API passphrase. This setting is only relevant for KuCoin.</p>
        <p>
          <br />In case you use a different trading key than your master key, make sure
          that both keys use the same passphrase.</p>
      </td>
    </tr>
  </tbody>
</table>### Trading Fees

{% tabs %}
{% tab title="Description" %}
This sets the trading fees paid to the exchange. Gunbot uses this data to calculate the break-even point.

Does your exchange charge 0.25% fees per trade? Then set this to 0.25. When your exchange has different fees for different types of trades, set the average fees per trade.

Trading fees are reflected in the average bought price. Exchanges only calculate fees after the trade comes in, Gunbot needs to know about fees before the trade is sent to the exchange.

{% hint style="info" %}
This parameter is irrelevant for trading at Bitmex.
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage

**Default value:** 0.25
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
| Stop limit | RT buy |
| RT buyback | RT sell |
|  | DCA buy |
|  | Close |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRADING_FEES`
{% endtab %}
{% endtabs %}



