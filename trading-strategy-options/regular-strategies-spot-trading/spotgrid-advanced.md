# Spotgrid Advanced

A grid like strategy with dynamic trading targets and integrated trailing for both buying and selling. 

Based on [spotGrid](spotgrid.md), with additional configuration options, trend detection and 'continuous trading'.

## Regular trading behavior

When you run spotGrid on a pair in an uptrend, this is the kind of trading behavior to expect:

![Typical spotGrid trades. Green arrows are buys, orange arrows are sells.](../../.gitbook/assets/image%20%28102%29.png)

Let's break down what actually happens:

* When the strategy first runs and there is no balance to sell, it immediately places a market buy order
* If price goes down and it exceeds the spotGrid line in the chart, buy trailing is activated. As soon as trailing finishes, a buy order is placed. The break even price is now lower.
* If price reaches the Sell target on the chart, sell trailing is activated and the complete position is sold at profit when trailing finishes.
* After having sold, the strategy immediately starts buy trailing. If price goes up it will quickly open a new position, if price goes down it will place a buy order below the last sell rate.

You will see that the first few buy orders happen fairly quickly when price starts going down. After a few trades, the price distance between buy orders gets much bigger. All targets are set automatically.

When the market goes down, the strategy starts to accumulate units at ever lower prices:

![Slowly accumulating with same sized orders](../../.gitbook/assets/image%20%2898%29.png)

The bot will continue to accumulate until max buy count is hit, or when it run out of available funds. Every new buy order will lower the break even price. As soon as price hits the sell target and finishes trailing above break even, a sell order is placed.

Balance management is very important, make sure you can afford the planned number of buy orders.

{% hint style="success" %}
The next trading targets are always visible on the chart. 

Keep in mind the targets lines are moving over time, they represent the current targets.
{% endhint %}

## Continuous trading behavior

CT, short for 'continuous trading', is a mechanism that lets you keep trading profitably even when the current price is far below the overall break even price. 

The principle is very simple: if price is above the price of the last buy order, up to the number of units bought in this trade can be sold for a small profit in base and in quote. The base profits you keep, the quote profits can help bring down break even of the overall position.

Besides the small profits from these partial trades, it allows your strategy to follow the market price more closely and sometimes fit in more DCA trades in one price range, compared to only waiting for prices to go down further to DCA.

![Continuous trading example](../../.gitbook/assets/image%20%28125%29.png)

The chart above shows several CT trades. Each sell order on this chart is only working with the units bought with the previous buy order. The number of units to 'CT sell' is configurable using the continuous trading limit multiplier, setting this to 1 would sell approx. the same number of units as last bought, 0.5 would sell approx. half of the units last bought. 

#### What happens when?

Assuming your settings allow CT orders:

* When price goes down after a DCA order: another DCA order gets placed when grid DCA target is hit.
* When price goes up after a DCA order: CT sell target is set above last buy rate, using 'gain' or 'auto gain'. When the target is reached, a CT sell order is placed.
* When price goes down after a CT sell order: if grid DCA target is reached, a 'CT buy' order is placed for approx. the same number of units as the CT sell order. The position size after this order is more or less identical compared to before the CT sell order.
* When price goes down after a CT buy order: a normal DCA order is placed when the DCA target hits.
* When price goes up after a CT sell order: if price is below break even and the distance between last sell rate and current price is more than the distance between first support and resistance, a new CT buy order gets placed.

{% hint style="info" %}
Manual trading or changing settings related to trading limit can disrupt continuous trading.

If you use CT, make it a habit to only change trading limit settings after a complete sell order.
{% endhint %}

## How to create a spotGridAdvanced strategy

* Using the advanced strategy editor: create a new strategy and select spotGridAdvanced as buy and sell method
* Using Easy Edit: create a new strategy and select spotGridAdvanced as buy method

## Essential settings

To use spotGridAdvanced, there are just three essential settings:

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
      <td style="text-align:left"><b>Max buy count</b>
      </td>
      <td style="text-align:left">
        <p><b>Limits how many buy orders are allowed.</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Setting a max buy count of 10 means that the bot may do up to 10 buy orders
          in a row, to average down in ever increasing price distance.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Min volume to sell</b>
      </td>
      <td style="text-align:left">
        <p><b>To ignore tiny balances. </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Set this to the minimum order size of the pair, to make the bot ignore
          small balances that cannot be sold.</p>
      </td>
    </tr>
  </tbody>
</table>

There are a few more settings that can optionally be used with this strategy, described below

## Additional settings

There are a few additional settings you can use with this strategy.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Stop after next sell                              </b>
      </td>
      <td style="text-align:left">
        <p><b>To stop trading a pair after the position closes</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>When enabled, no buy orders are placed when there is no balance left to
          sell.</p>
        <p>Averaging down of the current position continues.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Auto gain </b>
      </td>
      <td style="text-align:left">
        <p><b>Activates dynamic sell target </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Enabled by default. The dynamic target is based on the distance between
          support and resistance, capped at a minimum profit of 2x trading fees.
          The target decreases when position size increases. Overrules a manually
          set target.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Gain</b>
      </td>
      <td style="text-align:left">
        <p><b>Manual sell target value</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Set a manual sell target, as a percentage above the break even price.
          The sell target is where trailing starts. With 0.5 as value, the sell target
          is 0.5% above break even.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Grid multiplier</b>
      </td>
      <td style="text-align:left">
        <p><b>Controls the distances between buy orders </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Can be used to increase or decrease the distance between buy orders.</p>
        <p></p>
        <p>The default value 1 means that the hardcoded levels are used.</p>
        <p>1.5 would increase the levels with a factor 1.5x, a value of 0.5 would
          halve the default levels.</p>
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
      <td style="text-align:left"><b>SMA period</b>
      </td>
      <td style="text-align:left">
        <p><b>How many candles are used for calculating support and resistance levels </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>By default 50 candles are used to calculate support and resistance.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Keep quote</b>
      </td>
      <td style="text-align:left">
        <p><b>Keep a number of units when selling </b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>By setting keep quote at any number above 0, this number of units will
          be held back when the next sell order happens.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Trading limit multiplier</b>
      </td>
      <td style="text-align:left">
        <p><b>Controls the invest per DCA trade</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Can be used to increase or decrease the invested amount with each consecutive
          buy order. Examples with trading limit 100 USDT and max buy count 3:</p>
        <p></p>
        <p>1: each buy order is for 100 USDT</p>
        <p>1.5: first buy 100, second buy 150, third buy 225</p>
        <p>2: first buy 100, second buy 200, third buy 400&apos;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Trailing range multiplier</b>
      </td>
      <td style="text-align:left">
        <p><b>Controls how big trailing ranges are</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Can be used to increase or decrease the trailing range for all order types.</p>
        <p></p>
        <p>Default value 1 means that the hardcoded range is used. 1.5 would increase
          the range with a factor 1.5x, a value of 0.5 would halve the default trailing
          range</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Start continuous trading </b>
      </td>
      <td style="text-align:left">
        <p><b>Controls from which number of buy orders continuous trading is allowed</b>
        </p>
        <p>
          <br />A value of 3 means that when the position is 3x trading limit in size,
          the next sell target is placed above the last buy rate instead of above
          break even. Set a very high value to effectively disable this feature</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Continuous trading limit multiplier</b>
      </td>
      <td style="text-align:left">
        <p><b>Controls how much funds to use for CT</b>
        </p>
        <p>
          <br />Sets the ratio between last buy order invest in base, and the amount used
          for trades during continuous trading.</p>
        <p></p>
        <p>Value between 0 and 1. When set to 0.5, halve of the base amount used
          for the previous DCA trade will be used for continuous trading.</p>
        <p></p>
        <p>Setting 1 would use the same amount as the last DCA buy order</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Continuous trading restart multiplier</b>
      </td>
      <td style="text-align:left">
        <p><b>Sets the distance between a CT sell and a buy above that<br /></b>
          <br
          />Default is 1, should usually not be changed.</p>
        <p></p>
        <p>Examples:
          <br />1: buy target above last sell rate is placed once price is 1x the distance
          between sup/res above last sell rate</p>
        <p>0.5: buy target above last sell rate is placed once price is 0.5x the
          distance between sup/res above last sell rate</p>
      </td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
**Other parameters**

 Buy and Sell enabled options can be set as pair overrides. Watch mode is respected.

Besides settings mentioned on this page, no other strategy setting has any effect on spotGridAdvanced.
{% endhint %}

