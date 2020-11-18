---
description: >-
  Get your bot up and running, a brief guide / link collection for new Gunbot
  users.
---

# Quickstart guide

## 1. Download software

The latest stable version of Gunbot is always available on Github.

{% page-ref page="download.md" %}

## 2. Install & startup

The installation procedure is pretty straightforward, although there are slight differences between operating systems.

* Unpack the zip file to a new folder
* On Mac and Linux: set executable permission for the program
* Depending on your firewall: allow TCP traffic on the port used for the interface \(default port: 5000\)
* Start executable file and visit the interface in your browser on: [http://localhost:5000](http://localhost:5000)

{% page-ref page="windows.md" %}

{% page-ref page="macos.md" %}

{% page-ref page="linux.md" %}

{% page-ref page="arm.md" %}

{% hint style="info" %}
On Mac systems, expect to need to explicitly allow running various libraries used by Gunbot due to Gatekeeper restrictions.
{% endhint %}

## 3. Enter license data and connect an exchange

Before you can start trading with the bot, you'll need to enter a few things to make sure it will pass the license checks.

When you first start the interface, it will show a window that guides you through the initial setup of:

* Entering the wallet address where you hold your Gunthy tokens
* Connecting one exchange for which you've registered an API key after your order. Make sure to use the same API key as you've registered.
* Enter the fee percentage for the exchange you're connecting. This is used for properly calculating the break even point of every single trade the bot makes. If you're unsure, look it up the right percentage in the support docs of your exchange. On Binance \(spot\) the default fee level is 0.1.

![](../../.gitbook/assets/image%20%28108%29.png)

Save the settings after completing the wizard.

{% hint style="info" %}
Pick "other" if your registered exchange is not listed on the setup wizard.  
After saving, you can enter the exchange details on the profile page.
{% endhint %}

## 4. Add a trading pair and configure your first strategy

The easiest way to get started with strategies in Gunbot is using the **Easy edit** wizard. This guides you through the steps of adding a trading pair and configuring a strategy to execute.

![Adding a pair is as easy as picking the exchange and entering the pair name](../../.gitbook/assets/image%20%28110%29.png)

To configure the strategy, you can pick any of the included presets or enter a new name to create your own strategy.

![&quot;gain&quot; is an easy strategy to learn.](../../.gitbook/assets/image%20%28114%29.png)

Review your settings, then save the settings and hit the start trading button to run the bot you've just configured.

![Save the settings and start trading to run the bot on the pair you&apos;ve just configured.](../../.gitbook/assets/image%20%28111%29.png)

If you see a screen like this after starting trading, you're up and running. If the strategy has targets that can be translated to a price, you'd see the relevant targets right on the chart.

![&quot;Buy at&quot; is where a gain strategy will buy.](../../.gitbook/assets/image%20%28112%29.png)

## 5. Ready to go. Keep reading to get the most out of your bot

Your bot should now be up and running.

However, this is really just the beginning. There are a lot of strategy options, you should learn at least about the ones that will affect your trading strategy \(if you've picked one already\).

If you read through at least the following pages, you should quickly get a much clearer idea about how Gunbot actually works and how you can make it work for you.

### General information

{% page-ref page="../../how-to-work-with-gunbot/basic-workings/trading-logic.md" %}

{% page-ref page="../../how-to-work-with-gunbot/editing-strategies-and-pairs/trading-pairs/pair-processing.md" %}

{% page-ref page="../../how-to-work-with-gunbot/editing-strategies-and-pairs/" %}

### Essential info about strategies

{% page-ref page="../../how-to-work-with-gunbot/basic-workings/balance-settings.md" %}

{% page-ref page="../../trading-strategy-options/about-gunbot-strategies/" %}

