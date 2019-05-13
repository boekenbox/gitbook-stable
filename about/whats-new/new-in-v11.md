# New in v11

## New features

* **Huobi support:** Gunbot now supports trading at Huobi.
* **Improved indicators and charts:** tuning of indicators to better match TradingView, indicators shown in the GUI now match the indicator data Gunbot works with.
* **Mobile optimization:** the GUI is now optimized for use on mobile devices.
* **TV margin trading:** margin trading using the TradingView add-on is now supported at Huobi and Bitfinex.
* **Multiple base:** the new `MULTIPLE_BASE` lets you trade the same coin with multiple base currencies.
* **Override trading limit for TV:** specify the trading limit in buy alerts to override your config: BUY\_POLONIEX\_BTC-ETH\_0.01
* **Stop trading after a number of sell orders**: with `COUNT_SELL` you can disable trading after a configurable number of executed strategy sell orders.
* **Automatically disable bought price override:** you can now leave a `BOUGHT_PRICE` in place after you've set it, it is automatically disabled as soon as a valid price is available at the exchange.
* **Configurable buyback level for RT:** with `RT_BUY_UP_LEVEL` you can specify when to buy back quote units, in previous versions this was hardcoded to the break-even point.
* **Ichimoku:** implement `DISPLACEMENT`.
* **Improved portfolio value calculation:** portfolio value calculation no longer depends on a third party conversion service.
* **Stop limit / panic sell orders as market:** where supported, stop limit orders are now placed as market orders to increase the likelyhood of filling them asap.
* **Gain protection for all strategies:** every sell method can now optionally use gain protection.
* **Stepgain XTrend:** the use of XTrend to confirm orders with stepgain is now optional, trading without this confirmation roughly equals the legacy stepgain behavior.
* **Numerical DCA method:** Double Up can now be used without any indicators, working purely with price changes.
* **Never buy above:** only allow buy orders at a configurable percentage below the last sell rate.
* **New strategy presets:** every new installation now includes several new strategy presets.

## Bugfixes

Most critical issues were already patched in the bugfix releases for v10. Notable new fixes:

* Improved load times for the GUI on the dasbhoard and charts pages. Mitigating timeout errors during GUI load.
* Allow Bittrex International users to use Gunbot on the new platform.
* Several fixes for `KEEP_QUOTE`.
* Depreciating the separate RT process for stepgain. The normal RT process is now used.
* Fix several issues where partially filled orders would mess with RT.

