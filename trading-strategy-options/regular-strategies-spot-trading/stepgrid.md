# StepGrid

A grid like strategy that trades when price moves more than a defined step size. You can choose between using auto step size or setting a manual step size. Price trailing is completely automatic and happens every step up or down.

The idea of this strategy is to trade every meaningful price movement. Even while below the overall break even price of a position, it will still take advantage of small steps upwards to sell parts of a bag at rates profitable compared to their corresponding buy orders.

{% hint style="info" %}
This strategy is a bit different than most other Gunbot strategies: there are just a handful configurable options, confirming indicators are not a thing.
{% endhint %}

## Trading behavior

When you run `stepGrid` with auto step size, this is the kind of trading behavior to expect:

![Default stepGrid settings. Each arrow is an order: green = buy, orange = sell](../../.gitbook/assets/image%20%28129%29.png)

Let's break down what actually happens:

* Steps up and down are placed around last order rate, in case the trading pair has never traded before the reference price for steps is the price the pair had at the moment the strategy started running.
* If price moves down a step, it automatically starts buy trailing and places a buy order for 1x 'trading limit' as soon as trailing finishes.
* If price moves up a step while **below** break even, it automatically starts sell trailing and places a sell order when trailing finishes. Below break even each sell order is 1x 'trading limit' in size.
* If price is **above** break even, the behavior is the same but the order quantities can exceed 1x 'trading limit. If price reaches a full step above break even, it will close a bag at once.
* If price increases enough to reach the point where the next buy step is above break even, the buy step acts as stop in profit for positions bigger than 4x 'trading limit'.

When in no position, the bot will buy again when either the next buy or sell step is reached.

The bot will continue to accumulate until max buy count is hit, or when it run out of available funds. Every new buy order will lower the break even price.

Balance management is very important, make sure you can afford the planned number of buy orders.

{% hint style="success" %}
The next trading targets are always visible on the chart.

Keep in mind the targets lines are moving over time, they represent the current targets.
{% endhint %}

## How to create a stepGrid strategy

Using the advanced strategy editor: create a new strategy and select stepGrid as buy and sell method.

## Essential settings

To use stepGrid, there are just three essential settings:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Setting</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Trading limit</b>
      </td>
      <td style="text-align:left">
        <p><b>The amount to invest per buy order.</b>
        </p>
        <p></p>
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
        <p></p>
        <p>Setting a max buy count of 999 means that the bot may do up to 999 buy
          orders in a row.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Min volume to sell</b>
      </td>
      <td style="text-align:left">
        <p><b>To ignore tiny balances.</b>
        </p>
        <p></p>
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
      <td style="text-align:left"><b>Auto step size</b>
      </td>
      <td style="text-align:left">
        <p><b>Enables using automatic step size</b>
        </p>
        <p>Auto step size attempts to determine steps that just work.
          <br />When not in a position, auto step size is smaller than when in a position.</p>
        <p>When disabled, you can set a manual step size instead.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Step size</b>
      </td>
      <td style="text-align:left">
        <p><b>Sets a manual step size</b>
        </p>
        <p>Manual step size for buy and sell orders.
          <br />
          <br />Value represents price: setting 500 on USDT-BTC makes the bot trade each
          time price moves 500 USDT</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Enforce step size</b>
      </td>
      <td style="text-align:left">
        <p><b>Restricts buy and sell trailing to step size</b>
        </p>
        <p></p>
        <p>When enabled, price trailing is not allowed to trigger trades at rates
          less then the current step size. This ensures a minimum distance between
          orders.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Stop after next sell</b>
      </td>
      <td style="text-align:left">
        <p><b>To stop trading a pair after the position closes</b>
        </p>
        <p></p>
        <p>When enabled, no buy orders are placed when there is no balance left to
          sell.
          <br />
        </p>
        <p>Averaging down of the current position continues, partial sell orders
          do not make the bot stop afterwards.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Period</b>
      </td>
      <td style="text-align:left">
        <p><b>Sets the chart interval</b>
        </p>
        <p></p>
        <p>The default period is 15. Make sure to only use <a href="../../how-to-work-with-gunbot/basic-workings/period.md#supported-period-values">supported period values.</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>SMA period</b>
      </td>
      <td style="text-align:left">
        <p><b>How many candles are used for calculating support and resistance levels</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p>By default 50 candles are used to calculate support and resistance. In
          this strategy, support and resistance are used to determine appropriate
          trailing ranges</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>ATR period</b>
      </td>
      <td style="text-align:left"><b>How many candles are used for calculating ATR (average true range)<br /><br /></b>ATR
        is used for determining auto step size.
        <br />
        <br />By default it&apos;s set to use a 50 period ATR. Using lower values might
        cause step size to fluctuate too fast.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Keep quote</b>
      </td>
      <td style="text-align:left">
        <p><b>Keep a number of units when selling</b>
        </p>
        <p></p>
        <p>By setting keep quote at any number above 0, this number of units will
          be held back when the next sell order happens.</p>
      </td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
**Other parameters**

Buy and Sell enabled options can be set as pair overrides. Watch mode is respected.

Besides settings mentioned on this page, no other strategy setting has any effect on stepGrid.
{% endhint %}

