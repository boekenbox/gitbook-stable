# Tenkan

An easy to use strategy with built in DCA and ROE trailing, using the Ichimoku tenkan line.

{% hint style="info" %}
This strategy uses a limited subset of the regularly available strategy settings.   
Only the strategy options shown in the strategy editor for Tenkan have an effect.
{% endhint %}

## Trading behavior

A long position is opened when price is below kumo and tenkan crosses down the candle body.   
If the same happens again at a price at least 'DCA spread' below the previous order, a DCA order is placed.

A short position is opened when price is above kumo and tenkan crosses up the candle body.  
If the same happens again at a price at least 'DCA spread' above the previous order, a DCA order is placed.

To close a trade, the opposite conditions of opening it must happen and ROE scalper must finish trailing.

![Typical trades using the tenkan strategy](../../.gitbook/assets/image%20%28124%29.png)

## How to create a tenkan strategy

Use the advanced strategy editor and create a new strategy, select TENKAN as buy and sell method.



## Strategy settings

To use this strategy, these are the settings you can configure:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Trading limit                           </b>
      </td>
      <td style="text-align:left">
        <p><b>The amount to invest per buy order.</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>On an USDT-BTC pair, a trading limit of 20 means that every buy order
          is for 20 USDT. Always make sure that trading limit is set higher than
          min volume to sell.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Buy enabled</b>
      </td>
      <td style="text-align:left">
        <p><b>Allow / disallow buy orders</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>When true, the bot is allowed to place buy orders.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Sell enabled</b>
      </td>
      <td style="text-align:left">
        <p><b>Allow / disallow sell orders</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>When true, the bot is allowed to place sell orders.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ROE</b>
      </td>
      <td style="text-align:left">
        <p><b>Minimum ROE target</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>This is the minimum ROE target for any position.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ROE limit</b>
      </td>
      <td style="text-align:left">
        <p><b>Trailing range in ROE</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>When trailing, this setting defines how large the trailing range is.
          <br
          />
          <br />When set to 5 and trailing starts when 10% ROE is reached, the initial
          trailing stop will be set at ROE 5.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DCA spread</b>
      </td>
      <td style="text-align:left">
        <p><b>Minimum price difference between orders in the same direction</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>A DCA spread of 1 makes sure that orders are never placed closer than
          1% apart.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Maker fees</b>
      </td>
      <td style="text-align:left">
        <p><b>Option to use post only orders </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>With this option enabled, orders get placed as post only.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Stop limit</b>
      </td>
      <td style="text-align:left">
        <p><b>Target to close a trade at loss</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Setting 20 would close a position at loss when -20% ROE is reached.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Market close</b>
      </td>
      <td style="text-align:left">
        <p><b>Use market orders to close positions</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>When false, close orders are placed as limit orders. When true, close
          orders are placed as market orders.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Period</b>
      </td>
      <td style="text-align:left">
        <p><b>Sets the chart interval</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>The default period is 15. Make sure to only use <a href="../../how-to-work-with-gunbot/basic-workings/period.md#supported-period-values">supported period values.</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Candles length</b>
      </td>
      <td style="text-align:left"><b>Number of candles requested from the exchange</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Tenkan period</b>
      </td>
      <td style="text-align:left"><b>Number of candles used to calculate tenkan</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Kijun period</b>
      </td>
      <td style="text-align:left"><b>Number of candles used to calculate kijun</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Senkouspan period</b>
      </td>
      <td style="text-align:left"><b>Number of candles used to calculate senkou span</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Displacement</b>
      </td>
      <td style="text-align:left"><b>Number of candles to displace kumo</b>
      </td>
    </tr>
  </tbody>
</table>



