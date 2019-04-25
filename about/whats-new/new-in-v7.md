# New in v7

## Core changes

*  **New exchange**: Gunbot now supports trading on Binance.
*  **New strategies**: introduction of the Emotionless strategy. based on the Ichimoku cloud. This strategy "just works" and has no user configurable parameters, except tradings limits and the use of Double Up. Additionally, the Ichimoku strategy has been added.
*  **Improved speed and data usage**: more market data is saved offline, massively increasing cycling speed and reducing data usage.
*  **Secondary API key:** you can now activate a new API key without help from the support admins!
*  **Double Up improvements:** you can now limit the amount of times Double Up buys, choose between HIGH BB and RSI as triggers for averaging down and set a % below last bought price for Double Up buy orders to be placed.
*  **Easier pair syntax:** pair notation now has the same syntax for all exchanges.
*  **Funds reserve:** new parameter to keep an amount of base currency reserved at all times, not available for trades by Gunbot.
*  **RSI:** Adding RSI to \(almost\) all strategies, as an optional extra trigger for buying and/or selling.
*  **Gain:** new default values for Gain.
*  **TSSL**: various fixes.
*  **Stepgain**: improved strategy tuning.
*  **TradingView:** fixes for handling multiple incoming trading signals.
*  **Logs:** various log changes.
*  **Various:** Cache cleaning no longer triggers the safety switch.
*  **Various:** fix an issue that would fail to show base currency balance.
*  **Various:** many bugfixes for smaller issues.

##  GUI changes

*  **Pair syntax validation**: pairs added to a setup are now checked for proper syntax.
*  **Upgrades:** improved upgrade experience by the option to keep db.sqlite between versions
*  **TradingView charts:** fix charts for Kraken and Bitfinex. Disable charts for Cryptopia and Binance \(not supported by TradingView\).
*  **API Keys:** improved API key management.
*  **Various:** added version number to the interface.
*  **Various:** improved handling of wrong passwords.

