# Trading logic

## Gunbot does what you tell it to do

There is very little magic involved in this product, Gunbot tries to execute exactly the settings you feed it. 

This means that you pick the trading pairs the bot should trade and assign a trading strategy to these pairs. Once you've done that, all it takes is starting the bot core to begin automatic trading.

A trading strategy can be very simple, based on one of many presets - or you can create one from scratch using more than 120 different settings options.



## General principle: buy once, sell once

A strategy in Gunbot always consists of settings that control when it should buy, and when to sell.

Most strategies will buy one time once the buying criteria occur, then sell everything it bought as soon as selling conditions happen.

While true as a general principle, know that there are many exceptions to this - allowing for a whole lot of flexibility for traders:

* Some strategies can buy multiple times in a row
* Dollar Cost Avg \(DCA\) relies on buying at least twice
* Sell orders can sometimes be executed in multiple parts
* Sell orders can be configured to not sell all available funds
* Etc. This list is not meant as a complete overview of exceptions.



## Some settings have strategy specific behavior

Although many settings have exactly the same name across different buy & sell methods, they can still behave slightly different.

For every strategy there is full documentation on what every parameter does, read the strategy documentation if you are unsure about something.



## Changing settings on a running trading pair

You can change settings anytime, they take effect as soon as you save them.

If a trading pair placed a buy order with strategy A, it's perfectly fine to change a setting in strategy A or assign a different strategy to this pair. 



## Manual trading

It's generally not a problem at all if you trade manually on the same exchange account, or even the same pairs that Gunbot trades. Do make sure your settings for cancelling orders don't end up cancelling your manual orders though.

Gunbot will read from the trade history at the exchange and act accordingly. If you have multiple buy orders for a pair, it will work out the average bought price and use that as reference price for selling.

During Reversal Trading it is recommended to not manually trade the same pair.











