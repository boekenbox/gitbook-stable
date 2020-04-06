---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v18.8.5
---

# What's new?

Gunbot 18.8.5 offers much faster exchange connection on major exchanges, adds full support for Binance Futures. Includes bugfixes for known issues in the previous version.

## **Upgrading**

**There are no config changes for v18.8.5 In case you are upgrading from v18.x, replacing only the executable file** \(gunthy-win.exe / gunthy-linux / gunthy-macos / gunthy-arm\) **is all that's needed to upgrade.**

In case you are upgrading from an older version, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

\*\*\*\*[**Download release**](../../setup-and-general-settings/installation/download.md)\*\*\*\*

## Changes in v18.8.5

* **Support for websockets for major exchanges**: this allows for much lower delays on [supported exchanges](../supported-exchanges/websockets-support.md)
* **Full Binance Futures support:** Native margin strategies, TradingView alerts and Market Maker all work on Binance Futures
* **Support for Kraken forex trading pairs**
* **Implement Okex Futures:** Native margin strategies and TradingView alerts supported
* **New trading strategy:** Grid trading for margin / futures exchanges. Part of Market Maker plugin \(unlocked from token rank 10\)
* **Add Binance and Kraken support in Market Maker**

## Bugfixes

* Fix stop orders, \(fake\) market orders, order quantity  on Kraken Futures
* Fix for sometimes reverted trading history
* Disable post only for limit orders on Kraken Futures
* Solve problem with pSar for bitRage hedging
* Automatically switch to market orders for bitRage panic sell



\*\*\*\*

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

