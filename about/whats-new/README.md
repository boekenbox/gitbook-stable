---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v14.
---

# What's new?

* Empty your browser cache for your Gunbot GUI \(for example with ctrl-F5\).
* In case you used https for the GUI, also make sure to copy both the key and certificate files to the new folder.
* If you have pairs that are already in DU or RT, also copy the /json folder.
* New strategy parameters will be automatically added through the GUI the first time you update a strategy. This happens on a per strategy basis.
* Make sure to set the new Admin ID for Telegram, for better privacy.
* When you use email alerts, make sure the [syntax of your alerts](../../setup-and-general-settings/preferences/tradingview-add-on.md#alert-message-contents) is correct.
* If you also want to migrate your settings for the Telegram bot, copy the `tgconfig.json` file to your new folder as well.

Gunbot v13 focuses on bugfixes and improving user experience. That's why this time there is no long list of new features, just a long list of improvements.

## **Upgrading**

[Download](../../setup-and-general-settings/installation/download.md) the update and unpack it to a new folder, copy the `config.js` and `gunbotgui.db` files from your previous v12 installation into the new folder to keep your settings.

* Empty your browser cache for your Gunbot GUI \(for example with ctrl-F5\).
* In case you used https for the GUI, also make sure to copy both the key and certificate files to the new folder.
* If you have pairs that are already in DU or RT, also copy the /json folder from v12 to v13.
* New strategy parameters for v13 will be automatically added through the GUI the first time you update a strategy. This happens on a per strategy basis.
* Make sure to set the new Admin ID for Telegram, for better privacy.
* When you use email alerts, make sure the [syntax of your alerts](../../setup-and-general-settings/preferences/tradingview-add-on.md#alert-message-contents) is correct.
* If you also want to migrate your settings for the Telegram bot, copy the `tgconfig.json` file to your new folder as well.

_Due to new libraries used, you cannot simply overwrite the executable for this release._

\*\*\*\*

## New features / changes in v13

* **New exchanges:** Gunbot now supports trading on Idax and Cobinhood. Please note that Cobinhood only has an API key, not an API secret - use an IP restriction for your key for added security.
* **Restrict buying with `SMACROSS` :** you can now restrict `SMACROSS` to only place a single buy order, instead of buying on every cross up. Set `SINGLE_BUY` true to only allow a single buy order.
* **Improved privacy for Cryptosight:** restrict who is allowed to view and interact with your Telegram bot.  Use the `admin_id` setting to specify one or more user IDs that are allowed to use your Telegram bot.
* **Improved GUI template:** better optimized for mobile usage, different color schemes and support for setting a custom highlight color.
* **Changes to margin trading on Bitfinex \(TV add-on\):** all amounts should now be specified in quote. 
* **Different alert syntax for closing positions on Bitmex \(TV add-on\):** longs are now closed with CLOSELONG\_, shorts with CLOSESHORT\_ instead of CLOSE\_ for both.
* **New path for log files:** per pair log files are now saved in the /gunbot\_logs folder.

### Additional changes in patch release \(v14\):

* **Improved ROE calculation for Bitmex**: values now closely match the **leveraged ROE values on Bitmex** itself. If you're unsure what value to use in Gunbot, refer to the examples on the margin strategy pages for `ROE`.
* **TradingView alerts on Bitmex Testnet:** email alerts can now also be used on Testnet.
* **Close entire positions on Bitmex using alerts:** enable `TV_CLOSE_ALL` to close an entire position when a close alert arrives, instead of closing the amount specified in trading limit settings.
* **`SL_DISABLE_SELL` :** Disabled the sell side after a short position hit `STOP_LIMIT`.
* **Lending Rate \(Poloniex\):** Allow for entering a maximum lending rate for margin positions.

{% hint style="warning" %}
**Notice for Bitmex users**

Disable `STOP_LIMIT` by setting it's value to 9999, to prevent accidental triggers.

Because it now triggers on leveraged ROE value, it could be that a previous lower setting causes unwanted stops.
{% endhint %}

## **Gunbot core bugfixes**

Notable fixes:

* Better support for new currencies on Kraken, whatever shitcoin they are going to list, Gunbot will be ready to trade without needing an update
* Fix reversed orders array at Bittrex and Huobi
* Fix an issue that would cause multiple buys at Binance
* Fix an issue that would wrongly calculate trading map values on Binance
* Improve backward compatibility of ABP calculations
* Fix Huobi ABP
* Fix "lowerCase of undefined" error
* Improve SMACROSS behavior
* Cancel all stop orders if ROE is reached, on Bitmex
* Various fixes for manual orders through the GUI
* Fix buy/sell stop orders on Bitmex, for both market and limit orders.
* Fix STOP\_LIMIT sometimes not working on Kraken
* Fix stop orders triggering "waiting for open orders"
* Fix Tl\_ALLIN for long orders on Bitmex.
* Fix MACD-H display on custom strat
* Fix "toLowerCase" error
* Available funds are now used only after an RT\_SELL. 
* Fix FUNDS\_RESERVE not being respected
* Fix clientId issue on KuCoin
* If a buy gets canceled then set off double buy protection. 
* Correct an error that sometimes caused orders to be cancelled too early
* Improved triggering of DU orders
* Fix RT\_SELL, RT\_BUY triggering
* Fix "just bought"
* Fix price precision for CoinBase Pro

#### **Additional fixes in patch release \(v14\):**

* Fix rate NaN for some stop orders
* Fix invalid tickSize for limit orders at Bitmex with USD-ETH.
* Fix market sell orders with stop sell higher than zero. 
* Fix an issue that would keep stacking orders at Bitmex.
* Fix an issue that would send wrong quantity for market buy at Bitmex.
* Fix an issue that would silently fail to send sell market orders at Bitmex.
* Fix to "min volume not matched" message. 
* Fix tick size for stop orders. 
* Fix Huobi fiat pairs
* Fix stop order being sent at ask for stop buy.
* Show `MARKET_CLOSE` in the GUI if is margin strat. 
* Fix ROE\_TRAILING priority over ROE close. 
* Fix stop loss when a position is not even built yet because of exchanges delay. 
* Fix "just bought" not resetting, preventing further trades.
* Fix ROE close and ROE trailing.
* Fix send sell at ask and buy at bid, for certain trades.
* Various fixes for order rounding and price precision for different exchanges.

## TradingView add-on bugfixes

* Fix TV plugin LONG orders amounts
* Fix TV buy order at Bittrex
* Fx an issue that would prevent some buy orders at Bitfinex using TV plugin
* Fix CLOSELONG,CLOSESHORT for Bitfinex. All amounts for Bitfinex margin are now defined in quote
* Better respect order amounts in LONG and SHORT alerts on Bitfinex.
* Fix occasional errors for LONG,SHORT,CLOSELONG,CLOSESHORT on Bitmex.

## GUI changes

Notable bugfixes and changes to the GUI:

* New style and searchable dropdown selections
* Add GUI Tone color-picker in theme
* Fix navbar default active tab
* Fix imap tooltips
* Add themes for mobile
* Add Notification panel for mobile
* Update jquery cdn
* Major graphic update
* Improve mobile layout
* Redesign login page
* Update all the setting sections
* Improved tooltip texts
* Hide ROE\_CLOSE for strategies that don't use it

## Telegram changes

Notable changes to the Telegram bot:

* Fix notification profit percentage
* Hide wrong data in overview with multiple base
* Ability to restrict Cryptosight access to one or more Telegram users
* Fix creation/deletion of pairs. Changes are saved after going back to the main menu.
* Show contracts and liquidation price for margin

## GUI changes

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

