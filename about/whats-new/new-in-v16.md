---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v16
---

# New in v16

Gunbot v16 adds several new features and fixes known issues. 

## **Upgrading**

**There are no config changes for v16. In case you are upgrading from v15, replacing only the executable file** \(gunthy-win.exe / gunthy-linux / gunthy-macos / gunthy-arm\) **is all that's needed to upgrade.**

In case you are upgrading from v14, use the instructions below.

[Download](../../setup-and-general-settings/installation/download.md) the update and unpack it to a new folder, copy the `config.js` and `gunbotgui.db` files from your previous installation into the new folder to keep your settings.

* Make sure to set `TRADING_FEES` for each exchange, in the exchange section \(it changed from being a strategy setting to being an exchange setting. Either through the GUI or in `config.js`.
* Make sure to add the AutoConfig section to `config.js`, see the example config file in the v15 release files.
* Empty your browser cache for your Gunbot GUI \(for example with ctrl-F5\).
* In case you used https for the GUI, also make sure to copy both the key and certificate files to the new folder.
* If you have pairs that are in DU or RT, also copy the /json folder from your previous installation.
* When you use email alerts, make sure the [syntax of your alerts](../../setup-and-general-settings/preferences/tradingview-add-on.md#alert-message-contents) is correct.
* If you also want to migrate your settings for the Telegram bot, copy the `tgconfig.json` file to your new folder as well.

## New features / changes in v16

* **Support for specifying order rates for trades using TV add-on:** Limit orders can now be placed at a rate [specified in your alert](../../setup-and-general-settings/preferences/tradingview-add-on.md#alert-message-contents). 
* **More filter types for AutoConfig:** several [new ticker filters](../../how-to-work-with-gunbot/autoconfig.md#ticker-filters) for adding pairs with AutoConfig.
* **Backtesting option for Autoconfig:** simulate your filter settings for adding on historic ticker data.

\*\*\*\*

## **Gunbot core bugfixes**

Notable fixes:

* Fix several cases where a sell trailing range would no longer update. 
* Fixes for amounts for trades using the TradingView add-on
* Fix support for Yobit \(and likely many other exchanges with a similar issue\).
* Fixes for Kumex: prices, ROE trailing and UTA on \(MM only\).
* Use AUD-BTC as BTC-MFI pair on btcmarkets.
* Add support for M20 futures on Bitmex.





