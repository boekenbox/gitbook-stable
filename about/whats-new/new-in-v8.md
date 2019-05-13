# New in v8

## Core changes

* **New exchange:** Gunbot now support trading at CEX.io
* **Native Telegram trade notifications:** receive notifications on Telegram of every trade Gunbot places. Works on all exchanges and includes \(USD\) profit per trade, as well as current DU count.
* **Calculate indicator data each cycle:** with `TA_ALWAYS_ON` enabled, indicator data is calculated each cycle instead of each period. Only use this if you have unmetered bandwidth available.
* **Separate RSI level for DU:** with `RSI_DU_BUY` you can set a different RSI level for Double Up buy orders.
* **Timeout for order recalculation:** with `CANCEL_ORDERS_CYCLE_CAP` you can set the number of cycles to wait before Gunbot can cancel open oders.
* **Reserve pile up:** automatically add your profits to your funds reserve.
* **Optional: withdraw BTC profits** you can now automatically withdraw your BTC profits. USE THIS AT YOUR OWN RISK.
* **Pyramid buying for TV add-on** the TradingView add-on now support pyramid buying.

## GUI changes

* **Running multiple instances:** you can now run multiple setups from the GUI, each setup runs it's own Gunbot instance.
* **TradingView charts for Binance:** added support for loading TradingView charts for Binance pairs.
* **TradingView charts and basic stats on dashboard:** each pair on the dashboard now shows TradingView charts, basic Gunbot stats and trade notifications.
* **Improved onboarding:** it is now much easier to walk through the essential first steps of setting up Gunbot.
* **Various smaller improvements:** better grouping of items on the settings page, PM2 is no longer used, improved description texts for strategy parameters.

## Bugfixes

* Parsing Bittrex history using PricePerUnit \(Actual Price\) instead of Limit.
* Improved handling of promise rejections.
* Fix trading for coins with no recent trading history.
* Fix bittrex "available of undefined".
* Round down amounts to sell at Cryptopia \(workaround for an issue at Cryptopia\).
* Fix an issue that would trigger soft reboot too early.
* Allow panic sells during safety switch period.
* Fix "Already bought enough" issue for TradingView add-on.
* Fix Ichimoku strategy: buy/sell on TENKAN and KIJU crossing above and below KUMO \(confirmed by KUMO sentiments\).
* Fix Double Up if a partial sell is filled
* Fix an issue that would emit websocket for buy order before the order is actually placed
* Improved calculation of average bought price.
* Always log average bought price on console.
* Fix for trading at Kraken.
* Fix forever "waiting for open orders" issue

