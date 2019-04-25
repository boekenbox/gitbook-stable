---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v12.
---

# What's new?

## New features / changes

* **Bitmex support:** Gunbot now supports leveraged trading at Bitmex and Bitmex testnet. The new `PRE_ORDER` function lets you get ahead of the order book to increase the chance limit orders get filled. With `MAKER_FEES` all trades can be placed as post only orders, making Gunbot a market maker!
* **Margin trading strategies:** Strategies for Bitmex offer triggers to go long, short, stop and close on the desired ROE - or trail ROE up!
* **Blockchain license verification**: Using GUNTHY token, it is now possible to self-manage your API keys for free. 
* **Improved ichimoku:** Both the regular and margin version of the ichimoku strategy got a lot more powerful, you can now choose to open and close trades using either Tenkan-Sen, Kumo or Kijun-Sen.
* **Market orders:** Option to use market orders instead of limit orders, customizable for all order types. Works on exchanges that support Market orders: Binance, Bitfinex, Bitmex, Coinbase Pro, Kraken and Poloniex.
* **List view in GUI dashboard**: New list view for pairs on the dashoard: all relevant info in one quick overview, improves load times especially for setups with many trading pairs.
* **Chart improvements**: The GUI can now show all Gunbot supported indicators. Trading targets are now shown in charts.
* **Renko candles:** You can now use Renko candles instead of regular candle sticks \(Currently only works for margin trading with ichimoku\).
* **Deprecating** `CANCEL_ORDERS_CYCLE_CAP` **except when using** `MAKER_FEES`: orders are now only automatically cancelled if bid/ask are above/below the order rate.
* **Retain GUI access on startup errors**: Automatically stop Gunbot core when ran without pairs. If startup errors occur, a Telegram notification is sent and core gets stopped.
* **Dark theme for the settings page**: the settings page in the GUI is now dark.
* **Renamed executables**: the main executable files got renamed to be more coherent between operating systems.
* **Various small changes**

## **Bugfixes**

Many critical issues were already patched in the bugfix releases for v11. Notable new fixes:

* Fix an issue that would not save some settings changes made in the GUI.
* Work around Binance "minPrice" / "maxPrice" being 0.
* Improved handling of PAX & TUSD as base currency.
* Improved handling of sell orders at Binance.
* Fixes to SMACROSS, MACD and MACDH that prevented trades in specific conditions.
* Fix "RSI undefined" callback.
* Make sure a pair actually sold before letting `COUNT_SELL` disable it.
* Fix support for TUSD as base currency at Binance.
* Fix libraries for Raspberry Pi support.
* Reduced data/resource usage for the GUI.
* Workaround for an issue with Bitfinex not accepting close orders.
* Fix an issue that reset DU count after a partial sell order.
* Fix an issue with marging trading through TV add-on on Huobi.
* Various smaller fixes.

## **Upgrading**

Download the update and unpack it to a new folder, copy the `config.js` and `gunbotgui.db` files from your previous v11 installation into the new folder to keep your settings. In case you used https for the GUI, also make sure to copy both the key and certificate files to the new folder.

If you have pairs that are already in DU or RT, also copy the /json folder from v11 to v12.

New strategy parameters for v12 will be automatically added through the GUI the first time you update a strategy. This happens on a per strategy basis.

_Due to new libraries used, you cannot simply overwrite the executable for this release._

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

