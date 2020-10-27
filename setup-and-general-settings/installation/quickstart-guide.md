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

![If you see this after starting the bot, all is good and you can open the interface in your browser](../../.gitbook/assets/image%20%2860%29.png)

{% page-ref page="windows.md" %}

{% page-ref page="macos.md" %}

{% page-ref page="linux.md" %}

{% page-ref page="arm.md" %}

{% hint style="info" %}
On Mac systems, expect to need to explicitly allow running various libraries used by Gunbot due to Gatekeeper.
{% endhint %}

## 3. Enter license related data

Before you can start trading with the bot, you'll need to enter a few things to make sure it will pass the license checks.

When you first start the interface, it will show a window that guides you through the initial setup of:

* Entering the wallet address where you hold your Gunthy tokens
* Connecting one exchange for which you've registered an API key after your order. Make sure to use the same API key as you've registered.

![](../../.gitbook/assets/image%20%2863%29.png)

Save the settings after completing the wizard.

{% hint style="info" %}
Pick "other" if your registered exchange is not listed on the setup wizard.   
After saving, you can enter the exchange details on the profile page.
{% endhint %}

## 4. Ready to go. Keep reading to get the most out of your bot

Your bot should now be ready to start trading when you add a trading pair and assign it a strategy.

However, this is really just the beginning. There are a lot of strategy options, you should learn at least about the ones that will affect your trading strategy \(if you've picked one already\).

If you read through at least the following pages, you should quickly get a much clearer idea about how Gunbot actually works and how you can make it work for you.

### General information

{% page-ref page="../../how-to-work-with-gunbot/basic-workings/trading-logic.md" %}

{% page-ref page="../../how-to-work-with-gunbot/editing-strategies-and-pairs/trading-pairs/pair-processing.md" %}

{% page-ref page="../../how-to-work-with-gunbot/editing-strategies-and-pairs/" %}

### Essential info about strategies

{% page-ref page="../../how-to-work-with-gunbot/basic-workings/balance-settings.md" %}

{% page-ref page="../../trading-strategy-options/about-gunbot-strategies/" %}



