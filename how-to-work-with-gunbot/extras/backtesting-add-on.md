---
description: Gunbot strategies can be backtested on tradingview.com
---

# Backtesting

{% hint style="info" %}
**This is a paid add-on that works for Bitfinex, Binance, Bitmex, Bittrex, CEX, CoinBase Pro, Kraken and Poloniex.**

The Gunbot backtesting add-on is a simulation of Gunbot running inside a free or paid TradingView account. The indicator on TradingView is called Gunbot Deluxe Tuners BE \(Black Edition\).

This add-on is included with Gunbot Pro Edition or Gunbot Ultimate Edition purchased AFTER release of Gunbot version 10.

If you purchased one of these editions and did not receive this add-on please contact your reseller. You will need to provide your reseller with your TradingView profile name at time of purchase.
{% endhint %}

## Advantages

* Save time and learn Gunbot quickly by visually seeing how Gunbot's settings and strategies work.
* Optimize your settings for Gunbot through simulated backtesting on TradingView without risking your capital.
* Trade on alerts sent from the same tuned and backtested strategies at TradingView with the TradingView add-on.
* For learning, backtesting, and optimizing Gunbot with the simulated strategies script you will need either a free or paid TradingView account.
* If you wish to trade directly from the included alerts script you will also need the TradingView add-on and a paid TradingView account.

With the Gunbot TradingView add-on you can trade on alerts sent from custom strategies at Tradingview, completely managing your strategy at Tradingview. Gunbot receives trade signals by e-mail and trades accordingly.

{% hint style="info" %}
**Disclaimer**

While every effort has been made to ensure these simulations of Gunbot contain the same logic as Gunbot, they will not always buy or sell at the exact same time or prices as Gunbot \(because of TradingView's inability to use ticker prices\).

This is close as you can get in TradingView to the real thing. Backtesting the past does NOT guarantee profit in the present or future. Emotionless, Double Up, Reversal Trading, UTA, spotGrid \(and similar\) and tsa are not included due to limitations in TradingView.
{% endhint %}

## Getting started

Once you have purchased and provided your reseller with your TradingView profile name you will receive notification when your access has been granted and you may begin using in TradingView.

Login to your TradingView account and click the Chart button.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-1.png)

The scripts are best viewed in Dark theme.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-2.png)

Enter the Pair and the Period you wish to use and then click Indicators.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-3.png)

Do a search for "Tuners" and then click to load "Gunbot Deluxe Tuners BE" and "Gunbot Tuners Companion", you will only need these 2 for now. Later on you can return and load "Gunbot Deluxe Tuners BE \[Alerts\]" if you wish to send trade signals to Gunbot.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-4.png)

Now that you have the Tuner scripts loaded click the Strategy Tester to open it's pane.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-5.png)

Drag the top of the Strategy Tester pane to where you can see Net Profit and Buy & Hold Return.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-6.png)

Click the Gear icon to enter your Gunbot settings.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-7.png)

Enter your Exchange's TRADING\_FEES in the \[Properties\] Tab and then the \[Inputs\] Tab.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-8.png)

Drag this window to where you can also see the Strategy Tester pane stats.

![](../../.gitbook/assets/image%20%2862%29.png)

Now you are ready to begin adjusting settings in the \[Inputs\] window while watching Net Profit \(red is loss\) to see how this affects your returns. Once you have settings that you are satisfied with, record them to be entered into the real Gunbot when you are ready to begin trading that pair.

To see the "console" readings open the Data Window and hover mouse over a candle, this can be useful for troubleshooting and will also allow you to see the color coding of the plots \(inactive are blue\).

![](../../.gitbook/assets/image%20%2864%29.png)

The "Gunbot Tuners Companion" script is used for visually tuning the indicators which cannot be shown on the main chart window. It has it's own Gear icon where you will change it's settings. Once you have visually satisfactory settings, enter those same settings into the "Gunbot Deluxe Tuners BE" script settings to see how they perform.

![](../../.gitbook/assets/image%20%2856%29.png)

There are a few non-Gunbot variables that are also present in the scripts:

"BTC\_PND Exchange = " should be set to the Exchange of the pair you are testing if you are using BTC\_PND\_PROTECTION.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-12.png)

PricesA / PricesB sets percentage range of candle prices that are read. You may also choose to use candle sources instead. Only this portion of the candle will register when crossing indicators or levels. There is a plot in gray on top of the candles that represents this area.

```text
  |   High= 100
|   |
|   | PricesA = 0-100
|   |
|   | PricesB = 0-100
|   |
  |   Low = 0
```

Default range settings of 50 / 50 is equal to source settings of HL2 / HL2 for Prices.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-13.png)

TradingView scripts trade on the OPEN price of the NEXT candle AFTER conditions have been met. The green "LONG!" or "CLOSE SHORT!" and red "SHORT!" or "CLOSE LONG!" indicate the candle where the decision to trade was made, the blue, red, and purple markers indicate where the trade was made.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-14.png)

When using the "Gunbot Deluxe Tuners BE \[Alerts\]" script the green "LONG!" or "CLOSE SHORT!" and red "SHORT!" or "CLOSE LONG!" indicate on screen where the alerts will trigger and send to Gunbot's TradingView add-on.

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-15.png)

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-16.png)

![](https://raw.githubusercontent.com/boekenbox/gitbook-images/master/bt-17.png)

## FAQ

### **Why don't I see the fields for Double Up or Reversal Trading?**

Because Double Up and Reversal Trading is not supported \(as stated in Disclaimer above\).

### **Why can't I change the PERIOD from Auto?**

Because you don't need to, the script reads the PERIOD from whatever you set in the chart.

### **How do I link the Tuner to Gunbot?**

Currently there is not a way to export settings directly from Pine scripts to Gunbot. So once you find the best settings you have to copy and paste them one at a time into your config in Gunbot \(or open GUI / config.js beside TV script settings so you can see them to enter\).

### **What settings should I use in the Format: Properties Tab?**

Do NOT change any of these settings EXCEPT for Commission.

### **Why am I not seeing any trades?**

Because the pair is likely bagged up to the left of your screen. Temporarily lower the STOP\_LIMIT to a small value like 5 or scroll left to see. Other reasons may be that you have levels too conservative for that pair so no trade is ever made. **Temporarily lower BUY\_LEVEL, LONG\_LEVEL, and SHORT\_LEVEL to 0.1 to see.**

### **Why did it not trade where I think it should?**

Please refer to the Data Window while hovering over the candle in question to troubleshoot and answer.

**How far back can I test?**

This depends on the PERIOD you have set, the lower the PERIOD, the less amount of time can be backtested. There is a 2000 order limit in Pine. Free TradingView accounts provide 5000 candles of history and paid accounts provide 10000.

**Which browser should I use?**

Chrome, Edge, Safari browsers seems to work the best with TradingView, Internet Explorer and FireFox do not work as well with TradingView.

### **Can I use Heikin Ashi or other types of candles?**

No, the Tuner scripts only use regular style "Candles". Some of the other types will run and give results but they will not be accurate.

### **Why has my Tuner stopped working?**

If you encounter strange behavior first try zooming in or out, or refreshing the browser. If this does not solve then delete the indicator, refresh the browser page, and reload the indicator.

### **That didn't fix it, now what?**

If you continue to have issues: Format -&gt; Inputs -&gt; Defaults \(lower left\) -&gt; Reset Settings then remove the indicator, refresh browser, and reload it.

### **Still not working or I have a different question, so now what?**

Contact [@allanster](https://t.me/allanster) on Telegram.

