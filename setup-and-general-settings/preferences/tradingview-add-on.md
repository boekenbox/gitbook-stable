---
description: Learn how to use Gunbot to execute trades based on incoming email alerts.
---

# TradingView \(add-on\)

TradingView is the most active social network for traders and investors. TradingView allows users to create and share technical analysis and advanced trading strategies on their interactive charts.

With the Gunbot TradingView add-on you can trade on alerts sent from custom strategies at Tradingview, completely managing your strategy at TradingView. Gunbot receives trade signals by e-mail and trades accordingly.

This add-on can also be used for executing trades from arbitrary email alerts, TradingView is not strictly required.

{% hint style="info" %}
This is a paid add-on, availability depends on your license type.
{% endhint %}

## Setup video

Before you start setting up your alerts, you need:

* The IMAP data for the email address you receive alerts from TradingView on.
* A Pro subscription at tradingview.com \(works with trial too\). Free accounts are limited to one alert.

{% embed url="https://www.youtube.com/watch?v=H2EybuYxc3Y" caption="This video was made for an older Gunbot version. The basic steps still apply." %}

[Script used in example: Finn's Microprofit Strategy](https://gunthy.org/forum/index.php/topic,1548.0.html)

{% page-ref page="imap-listener.md" %}

## Alert message contents

The alerts messages have to be in the following format in order for Gunbot to act on them. Alerts follow the same standardized pair syntax that also apply for normal Gunbot usage.

Trading limits can only be specifically defined in buy/long alerts, for other alerts or alerts without specified amounts, the limits as set in your TradingView settings apply.

_Replace_ `EXCHANGE` _with the name of your exchange._

\_\_

#### For all spot exchanges

<table>
  <thead>
    <tr>
      <th style="text-align:left">Alert message</th>
      <th style="text-align:left">Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BUY_EXCHANGE_BTC-ETH</td>
      <td style="text-align:left">Buy ETH using BTC</td>
    </tr>
    <tr>
      <td style="text-align:left">BUY_EXCHANGE_BTC-ETH_0.1</td>
      <td style="text-align:left">Buy ETH using BTC with a trading limit of 0.1 BTC</td>
    </tr>
    <tr>
      <td style="text-align:left">SELL_EXCHANGE_USDT-BTC</td>
      <td style="text-align:left">Sell BTC for USDT</td>
    </tr>
    <tr>
      <td style="text-align:left">SELL_EXCHANGE_USDT-BTC_0.1</td>
      <td style="text-align:left">Sell USDT for BTC with a trading limit of 0.1 BTC</td>
    </tr>
    <tr>
      <td style="text-align:left">STOPLOSS_EXCHANGE_BTC-ETH</td>
      <td style="text-align:left">Sell ETH for BTC if stoploss is triggered</td>
    </tr>
    <tr>
      <td style="text-align:left">BUY_EXCHANGE_USDT-BTC_amount_rate</td>
      <td style="text-align:left">
        <p>Buy BTC using USDT for a specified amount</p>
        <p>in USDT at a specified rate.</p>
        <p>Requires TV market orders to be disabled</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BUY_EXCHANGE_BTC-ETH_0_rate</td>
      <td style="text-align:left">
        <p>Buy BTC using USDT for &quot;TV trading limit buy&quot;,</p>
        <p>at a specified rate.</p>
        <p>Requires TV market orders to be disabled.</p>
      </td>
    </tr>
  </tbody>
</table>

#### Alerts for margin trading

<table>
  <thead>
    <tr>
      <th style="text-align:left">Alert message</th>
      <th style="text-align:left">Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Short order for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Long order for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD_amount</td>
      <td style="text-align:left">Short order for XBT-USD with a specified trading limit</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD_amount</td>
      <td style="text-align:left">Long order for XBT-USD with a specified trading limit</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD_amount_rate</td>
      <td style="text-align:left">
        <p>Long order for XBT-USD with a specified</p>
        <p>trading limit and rate.</p>
        <p>Requires TV market orders to be disabled.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD_0_rate</td>
      <td style="text-align:left">
        <p>Short order for XBT-USD without a specified</p>
        <p>trading limit and with a specified rate.</p>
        <p>Requires TV market orders to be disabled.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">CLOSELONG_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Closes a long position for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">CLOSESHORT_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Closes a short position for XBT-USD</td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
**Note about trading limits**

On **Bitmex**, every setting related to trading limits for margin trading must be specified in contracts.   
  
On **all other supported margin exchanges**, every setting related to margin trading limits must be specified in amounts of quote currency.
{% endhint %}

{% hint style="success" %}
To test alerts on Bitmex Testnet, you should write the alerts like this: LONG\_BITMEXTESTNET\_XBT-USD
{% endhint %}

## TradingView settings

To run Gunbot with the TradingView add-on, the following are the only relevant settings. Normal Gunbot strategy and pair settings are not relevant and not used unless `TV_GB` is enabled.

Open the settings by going to **Settings** &gt; **TradingView.**

![Settings options for the TradingView add-on](../../.gitbook/assets/image%20%2850%29.png)

Trading limits for buy orders are set in the configuration settings, optionally you can override these by specifying the trading limit in the alert message contents.

Orders placed by the TradingView add-on are placed by default as market orders to ensure they will get filled. Optionally, you can let it send limit orders as well. 

{% hint style="info" %}
Be sure to add one pair for any exchange you want to execute alerts on.

This can be any pair, it will not be used by the add-on.
{% endhint %}

### Gain

{% tabs %}
{% tab title="Description" %}
Set a minimum gain in % that trades initiated by TradingView must comply to when `TV_PROTECTION` is enabled.

When TradingView sell alerts are sent that would have a lower gain than this value, Gunbot will not place the order. Use this to prevent selling at loss.

{% hint style="warning" %}
**Only works for spot trading.**
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a percentage.

**Default value:** 0.6
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_GAIN`
{% endtab %}
{% endtabs %}

### Market orders

{% tabs %}
{% tab title="Description" %}
By default, orders are sent as market orders.

When you want to send limit orders instead, disable this option. Buy orders are then sent at a limit price of the best bid order in the order book, sell orders are placed at the rate of the best ask price.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_MARKET_ORDERS`
{% endtab %}
{% endtabs %}

### Trading Limit Buy

{% tabs %}
{% tab title="Description" %}
This value defines the trading limit for each buy order placed through the add-on.

The default value of 0.002 would place orders of 0.002 BTC when used on a BTC-x pair.

When not using `TV_PYRAMID`, a sell alert will place a sell order for the full quote balance held.

{% hint style="info" %}
**Bitmex**: enter the desired number of contracts

**Other margin exchanges:** enter an amount in quote currency
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.002
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_TRADING_LIMIT_BUY`
{% endtab %}
{% endtabs %}

### Trading Limit Buy Pyramid

{% tabs %}
{% tab title="Description" %}
This value defines the trading limit for each pyramid buy order placed through the add-on.

The default value of 0.002 would place orders of 0.002 BTC when used on a BTC-x pair.

{% hint style="info" %}
**Bitmex**: enter the desired number of contracts

**Other margin exchanges:** enter an amount in quote currency
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.002
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_TRADING_LIMIT_BUY_PYRAMID`
{% endtab %}
{% endtabs %}

### Pyramid

{% tabs %}
{% tab title="Description" %}
Setting this to true enables pyramid trading, the amount for each pyramid order is defined by `TV_TRADING_LIMIT_SELL` or `TV_TRADING_LIMIT_BUY_PYRAMID`.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_PYRAMID`
{% endtab %}
{% endtabs %}

### Trading Limit Sell

{% tabs %}
{% tab title="Description" %}
This value defines the trading limit for sell orders when `TV_PYRAMID` is enabled.

The default value of 0.002 would place orders of 0.002 BTC when used on a BTC-x pair.

{% hint style="info" %}
**Bitmex**: enter the desired number of contracts

**Other margin exchanges:** enter an amount in quote currency
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.002
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_TRADING_LIMIT_SELL`
{% endtab %}
{% endtabs %}

### Protection

{% tabs %}
{% tab title="Description" %}
When set to true Gunbot will check if there is an overall profit before selling, as specified in `TV_GAIN`.

When set to false, Gunbot will execute all TradingView alerts without interfering with a custom strategy.

{% hint style="warning" %}
**Only works for spot trading.**
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_PROTECTION`
{% endtab %}
{% endtabs %}

### Close all

{% tabs %}
{% tab title="Description" %}
When enabled, an entire position will be closed upon receiving a close command - even if the position is bigger than the set trading limits.

{% hint style="warning" %}
**Only works for Bitmex.**
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_CLOSE_ALL`
{% endtab %}
{% endtabs %}

### Trading Limit Cap

{% tabs %}
{% tab title="Description" %}
The maximum amount of base currency to be invested in a pair.

{% hint style="warning" %}
**Only works for spot trading.**
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents an amount in base currency.

**Default value:** 0.0001
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_TRADING_LIMIT_CAP`
{% endtab %}
{% endtabs %}

### Stoploss Percentage

{% tabs %}
{% tab title="Description" %}
Percentage below average bought price at which a sell signal should override `TV_PROTECTION` and sell in a stop-loss manner.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents a percentage.

**Default value:** 60
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_STOPLOSS_PERCENTAGE`
{% endtab %}
{% endtabs %}

### Trading Limit All-In

{% tabs %}
{% tab title="Description" %}
When set to true, each buy order will use all available base currency balance.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_TRADING_LIMIT_ALLIN`
{% endtab %}
{% endtabs %}

### Retry Order

{% tabs %}
{% tab title="Description" %}
Enable this when you have problems receiving multiple alerts.

Gunbot will retry processing orders for 15 minutes.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `RETRY_TV_ORDER`
{% endtab %}
{% endtabs %}

### TV MVTS

{% tabs %}
{% tab title="Description" %}
Sets a threshold for sell orders, If you own less than the set amount, sell orders will not be placed and the bot goes into buying mode again.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represents the total value of a coins holdings in base currency.

**Default value:** 0.001
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_MVTS`
{% endtab %}
{% endtabs %}

### TV GB

{% tabs %}
{% tab title="Description" %}
Enable this to run Gunbot strategies simultaneously with the TradingView add-on. This way buying and selling with Gunbot strategies or TradingView alerts can be mixed.

The IMAP listener needs to be enabled to use this option.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_GB`
{% endtab %}
{% endtabs %}

### TV Leverage

{% tabs %}
{% tab title="Description" %}
For margin trading only. Sets the leverage for opening any position. Setting 0 places the order with cross margin, if your exchange supports cross leverage.  
  
Only use values supported by your exchange.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – between 0 and 100

**Default value:** 0
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_LEVERAGE`
{% endtab %}
{% endtabs %}

### TV Lending

{% tabs %}
{% tab title="Description" %}
Sets the maximum acceptable lending rate when opening a position. 

The default setting of 0.02 stands for 2% interest per day.

{% hint style="info" %}
**Only works on Poloniex.**
{% endhint %}
{% endtab %}

{% tab title="Values" %}
**Values:** numerical 

**Default value:** 0.02
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TV_LENDING`
{% endtab %}
{% endtabs %}



