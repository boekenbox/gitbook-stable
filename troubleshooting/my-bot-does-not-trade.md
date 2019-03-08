# Bot does not trade

{% hint style="info" %}
A common question among new users is "why does my bot not trade? 

There could be complicated reasons why this is the case, but often it's really simple. 
{% endhint %}

## Checklist if your bot does not trade

Before you ask for help, go over this checklist:

* Verify that your bot is correctly running. If you see a "grid" output with trading data in the console window, then your bot is usually correctly processing pairs. 
* Make sure that you have the **Watch Mode** option disabled.
* Make sure that your trading strategy has both buying and selling enabled. You can find these settings in the Strategy Configurator.
* Double check that the [essential balance settings](../how-to-work-with-gunbot/basic-workings/balance-settings.md) are set correctly, above the exchange minimum trade size.
* Try reducing the setting for **Buy Level**. The default setting of 1 can be very restrictive on some pairs or days where the market does not move much.



