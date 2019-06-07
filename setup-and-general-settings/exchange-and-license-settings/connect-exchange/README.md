# Connect exchange

To be able to trade, you need to enter the exchange [API key](creating-api-keys.md) and secret.

To enter these, go to **Settings &gt; Trading &gt; Exchanges**.

![](https://user-images.githubusercontent.com/2372008/52206416-707ef380-287a-11e9-8f93-881abdbe9e5b.png)

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
      <td style="text-align:left"><b>password</b>
      </td>
      <td style="text-align:left">
        <p>Your KuCoin trading passphrase. This setting is only relevant for KuCoin.</p>
        <p>
          <br />In case you use a different trading key than your master key, make sure
          that both keys use the same passphrase.</p>
      </td>
    </tr>
  </tbody>
</table>