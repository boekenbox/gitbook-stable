# New in v9

## Core changes

*  **GDAX support:** Gunbot now supports trading at GDAX.
*  **Reversal trading:** Enables trading in downtrends by accumulating quote currency without investing extra base. **Do read up on the wiki before you enable this.**
*  **New indicators:** You can now use the Stochastic oscillator and per pair MFI as additional confirmation in all strategies.
*  **TrailMe:** Most strategies can now use TSSL-style trailing. Additionally, you can use trailing for reversal trading and double up.
*  **Custom strategies \(beta\):** Introducing custom strategies. In this first release of custom strategies it is possible to define your own strategy based on SMA-cross, MACD or MACDH - optionally combined with MFI, RSI or Stochastic.
*  **Trading fees:** You can now define the fees Gunbot needs to account for in `TRADING_FEES`. `GAIN` is now a percentage above the expected fees paid.
*  **Pair suggestions:** Based on trading volume and volatility, Gunbot suggests possible good pairs to trade on.
*  **Automatic retry for TV orders:** Implementing `RETRY_TV_ORDER` to automatically retry orders when processing multiple alerts.
*  **TradingView:** Introducing margin trading at Poloniex. All buy orders are now placed as market orders.
*  **Dust handling:** `MIN_VOLUME_TO_SELL` is expressed in base currency again.

##  GUI changes

*  **Performance improvements:** much better responsiveness, reduced CPU load and memory usage.
*  **New design:** new dark color theme, various design refinements.
*  **Native charts:** charts based on the same data Gunbot processes, including trades visualization.
*  **Log rotation:** automatic rotation of out.log files. No more "empty logs" errors.

##  Bugfixes

*  **Fixes for multiple orders on TV:** improved handling of multiple incoming signals.
*  **Kraken:** Multiple fixes for Kraken.
*  **Bittrex:** Fixing "Available of undefined" error.
*  **Emotionless** Fixing various issues with Emotionless.
*  **TSSL:** Prevent selling while price is still in range. Preventing buy orders above lowest EMA.
*  **"Waiting for open orders":** Fixing an issue where the bot would wait for open orders, even if orders got deleted.
*  **RSI calculation:** improved RSI calculation when `OKKIES_MODE` is disabled.
*  **Kraken:** Fix for Kraken candles.
*  **Unable to check master key":** Better error handling. The \*Unable to check master key" now only occurs on local errors \(wrong pair, wrong api key, etc.\).
*  **Binance:** using api.binance.com instead of [www.binance.com](http://www.binance.com)
*  **Reduce data usage and API calls:** dramatically reduce the amount of API calls and data usage while still updating indicators each cycle. Deprecating `TA_ALWAYS_ON`.
*  **TV charts \(GUI\)** showing charts and pair data faster on the dashboard.
*  **Dashboard:** Fixing order notifications with a volume of 0.

