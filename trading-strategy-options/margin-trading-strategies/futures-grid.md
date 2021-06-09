# Futures Grid

A grid like strategy with dynamic trading targets and integrated trailing for both buying and selling.

The idea of this strategy is to always be in a position, use market orders only, average down when prices goes down, take profit when price exceeds break even. And do so with an absolute minimum ****number of settings.

This strategy is basically the same as "spotGrid", it's just made for futures and additionally offers the option to trade shorts.

{% hint style="info" %}
This strategy is a bit different than all other Gunbot strategies: there are just a handful configurable options and it's "always in position": instead of waiting for the perfect entry it is always looking for chances to average down.  
  
If you want to customize settings a lot, this strategy is not for you.
{% endhint %}

## Trading behavior

When you run futuresGrid on a pair in an uptrend, this is the kind of trading behavior to expect:

![Typical futuresGrid trades. Green arrows are buys, orange arrows are sells.](../../.gitbook/assets/image%20%28102%29.png)

Let's break down what actually happens:

* When the strategy first runs and there is no balance to sell, it immediately places a market buy order
* If price goes down and it exceeds the futuresGrid line in the chart, buy trailing is activated. As soon as trailing finishes, a buy order is placed. The break even price is now lower.
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

## How to create a futuresGrid strategy

* Using the advanced strategy editor: create a new strategy and select futuresGrid as buy and sell method



## Essential settings

To use futuresGrid, there are just three essential settings:

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
        <p><b>The amount to invest per buy order</b>
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
        <p><b>Limits how many buy orders are allowed</b>
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
        <p><b>To ignore tiny balances</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>Set this to the minimum order size of the pair, to make the bot ignore
          small balances that cannot be sold.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Max open contracts                      </b>
      </td>
      <td style="text-align:left">
        <p><b>Limits position size</b>
        </p>
        <p>
          <br />On an USDT-BTC pair, setting max open contracts to 0.1 means that no buy
          order gets fired if it would result in the total position exceeding 0.1
          BTC.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Direction</b>
      </td>
      <td style="text-align:left">
        <p><b>Sets the direction to trade</b>
        </p>
        <p></p>
        <p>Pick either <code>LONG</code>, <code>SHORT </code>or <code>AUTO</code>
        </p>
        <p></p>
        <p>The AUTO mode uses a non configurable trend detection that uses EMA, OBV
          and ATR data from both 15m and 4h charts, and only opens a long or short
          if confirmed by the trend definitions.</p>
        <p></p>
        <p>If in a position already and trend changes, the strategy will first attempt
          to close at profit before opening a position in the opposite direction.</p>
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
        <p>When enabled, no new position gets opened.</p>
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
      <td style="text-align:left"><b>Trading limit multiplier</b>
      </td>
      <td style="text-align:left">
        <p>Can be used to increase or decrease the invested amount with each consecutive
          buy order. Examples with trading limit 100 USDT and max buy count 3:</p>
        <p></p>
        <p>1: each buy order is for 100 USDT</p>
        <p>1.5: first buy 100, second buy 150, third buy 225</p>
        <p>2: first buy 100, second buy 200, third buy 400</p>
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
  </tbody>
</table>

{% hint style="info" %}
**Other parameters**

 Buy and Sell enabled options can be set as pair overrides. Watch mode is respected.

Besides settings mentioned on this page, no other strategy setting has any effect on futuresGrid.
{% endhint %}

