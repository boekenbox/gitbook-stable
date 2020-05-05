---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v16.7+
---

# New in v16.7+



Gunbot v16**.**9 adds several new features and fixes known issues. 

## **Upgrading**

**There are no config changes for v16.9 In case you are upgrading from v16, replacing only the executable file** \(gunthy-win.exe / gunthy-linux / gunthy-macos / gunthy-arm\) **is all that's needed to upgrade.**

In case you are upgrading from an older version, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

\*\*\*\*[**Download release**](../../setup-and-general-settings/installation/download.md)\*\*\*\*



## New features / changes in v16.9

* **Support for our exchange:** [**Gunthy**](https://exchange.gunthy.org)\*\*\*\*
* **New partner exchange:** Binance
* **New Autoconfig features:** 
  * New filter: linear regression channel
  * Option to filter on pair variables from another exchange
* **Support for Bittrex credits:** automatically used when enabled at Bittrex and there is a balance for BTXCRD



## New features / changes in v16.7

* **New partner exchange:** Kraken Futures partnered with Gunbot and is now fully supported in Gunbot.
* **Many new Autoconfig features:** 
  * Reduce average processing time for jobs with ticker filters.
  * Add an option to reset user variables, without having to create a separate job for it.
  * New filter types: 
    * bearishStandardDeviationChannel \(+ \*History variants\)
    * many time based filters
    * filters based on last order type for specific pairs
    * strategy name filter
    * variableNotExist: to check if a variable is not set
    * allowsHedging: only add pairs that allow for direct hedging between a specified hedge pair
  * Per pair user variables, allowing you to create pair specific interactions between jobs.
  * Option to disable Telegram notifications per job.
  * Option to change exchange delay with a manageOverrides job.
  * New job types:

    * manageBotSettings: to make changes in global bot settings
    * hedgeGB & hedgeGB2 allow for hedging with Gunbot

  

## **Gunbot core bugfixes**

Notable fixes:

* Fix UTA for H20/M20 pair on Bitmex.
* Better tuning for timely fetching Kucoin tickers.
* Fix many profit calculations in CryptoSight, fix admin rights issue.
* Add support for ALGO and USDC pairs on Kraken.
* Fix TV being stuck after an incoming close alert when there is no position to close.
* Better handling of DU count.
* Fix an issue with TakeProfit that sometimes made it falsely trigger when prices moved up.
* Autoconfig fixes: 
  * Fix backtesting for jobs using \*History filters.
  * Fix issue with processing Huobi tickers in case new pairs are introduced on the exchange.
  * Better handling of removing the last pair on an exchange.
  * Fix parsing of markdown elements in telegram alerts.
  * Automatically purge old history data when history value in config gets reduced.
  * Also look for mvts value in pair overrides.
* Various fixes that improve performance on Kraken Futures

