---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v15.
---

# New in v15

Gunbot v15 is a major release: it includes support for over 100 new exchanges - including Binance US and HitBTC - and a pre release of AutoConfig.

## **Upgrading**

[Download](../../setup-and-general-settings/installation/download.md) the update and unpack it to a new folder, copy the `config.js` and `gunbotgui.db` files from your previous installation into the new folder to keep your settings.

* Make sure to set `TRADING_FEES` for each exchange, in the exchange section \(it changed from being a strategy setting to being an exchange setting. Either through the GUI or in `config.js`.
* Make sure to add the AutoConfig section to `config.js`, see the example config file in the v15 release files.
* Empty your browser cache for your Gunbot GUI \(for example with ctrl-F5\).
* In case you used https for the GUI, also make sure to copy both the key and certificate files to the new folder.
* If you have pairs that are in DU or RT, also copy the /json folder from your previous installation.
* When you use email alerts, make sure the [syntax of your alerts](../../how-to-work-with-gunbot/extras/tradingview-add-on/#alert-message-contents) is correct.
* If you also want to migrate your settings for the Telegram bot, copy the `tgconfig.json` file to your new folder as well.

## New features / changes in v15

* **Over 100 new supported exchanges:** Gunbot now [supports](../supported-exchanges/) spot trading on all exchanges supported by the [ccxt library](https://github.com/ccxt/ccxt/wiki/Exchange-Markets)! This includes support for email alerts through the TradingView add-on.
* **Trading fees setting moves to exchange section:** for easier strategy handling, the TRADING\_FEES setting is now configurable per exchange. It's no longer a strategy setting.
* **Limit orders for TradingView add-on:** optionally you can now let the TradingView add-on place limit orders instead of market orders.
* **Pre-release of AutoConfig:** with [AutoConfig ](../../how-to-work-with-gunbot/extras/autoconfig.md)you can automate all kinds of things you previously could only do by hand. For example you can use it automatically add high volume trading pairs, assign a bag handling strategy for pairs that are not performing well, or automatically update pair overrides. It's called a pre-release because there no GUI for it yet, it's fully functional though.

## **Gunbot core bugfixes**

Notable fixes:

* Bring back Telegram integration for the ARM edition.
* Porting many small exchange fixes from bitRage to Gunbot.
* Fix an issue that could cause unwanted restarts on Poloniex.
* Fix "bought already" problem that prevented trading for some users.
* Fix two long standing bugs that caused trailing to sometimes not trigger an order.
* Fix an issue on Kraken and Bittrex that caused all pairs to wait when there was an open order for a single pair.
* Fix an issue that could sometimes invert ROE on Bitmex.
* Fix for TV stoploss alerts.

