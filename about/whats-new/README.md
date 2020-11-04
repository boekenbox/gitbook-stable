---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v20
---

# What's new?

Gunbot v20 introduces a completely new interface built around the TradingView charting library, with easier options to create and edit strategy settings. Also comes with a ton of other changes and stability improvements.

## **Upgrading**

{% hint style="success" %}
**New: config import tool**

It's now really easy to upload a config.js or autoconfig.json file in the interface, and import the parts you need from it. You can use this to setup a clean v20 installation while migrating your old pairs, strategies and other settings.  
  
**Steps to start with a clean installation:** unpack v20 and run, enter your Gunthy wallet address in the wizard and select "other" as exchange and save the changes. Then go to _Profile_ &gt; _Import settings_ and select the parts from your old config to import.
{% endhint %}

There are no breaking config changes for v20. In case you are upgrading from v18.x, replacing the executable file _\(gunthy-win.exe / gunthy-linux / gunthy-macos\)_ and _node\_modules_ folder is all that's needed.

![The GUI requires that password authentication is enabled in config.js. Set it like this.](../../.gitbook/assets/image%20%2874%29.png)

New strategy parameters are automatically merged into your strategies, as soon as you login to the browser interface.

In case you are upgrading from an older version than v18, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

## Changes in v20

Only the most important changes are listed.

### Core / GUI

* **Completely new GUI:** improved performance, easier to use, better charting, improved trading terminal, more stats, additional config wizard for novice users
* **Seamless config changes:** pair cycling is no longer interrupted when a config change happens. New settings take effect the next cycle
* **New partner exchanges:** Bitget and Nash
* **New strategy for spot trading**: [Support / Resistance](../../trading-strategy-options/regular-strategies-spot-trading/support-resistance.md)
* **PND close**: an alternative to ROE trailing to aim for bigger profits in margin strategies. PND attempts to wait until a move plays out before closing position
* **Alternate ROE trailing method  for margin strats:** with `ROE_SCALPER` the trailing range is an absolute ROE value
* **Liquidity maker for spot trading:** provide [liquidity ](../../trading-strategy-options/misc-settings.md#liquidity-maker)and profit from bid/ask spread
* **Easier config sharing**: import parts of `config.js` or `autoconfig.json` using the GUI
* **Store more order history:** to improve profit/loss calculations and prevent unneeded "bought price" warnings, the complete order history is now locally saved over time 
* **TradingView alerts**: option to change strategy by alert, enable `TV_TRADING_LIMIT_CAP` for Bitmex, better handling of multiple incoming emails
* **Alerts \(beta\):** build your own strategy in a visual way using built-in TradingView charts

### Market Maker

* **New strategy variants**: Grid, Support/Resistance, Fibonacci, Pullback, One Scalper, x125 & Moto

### AutoConfig

* **Editor and stats in the GUI**: every single AutoConfig feature is now editable through the GUI, every config change is visible on the AutoConfig dashboard
* **More filter types**: add pairs with linear regression filters, more [options ](../../how-to-work-with-gunbot/extras/autoconfig.md#generic-filters)to filter your own variables
* **Use your own logic:** most values in Autoconfig jobs can now optionally use [custom JavaScript expressions](../../how-to-work-with-gunbot/extras/autoconfig.md#calculated-config-values-and-custom-filters). Expressions have access to almost all internal bot data
* **Silent mode**: disable console logs for AutoConfig

### Bugfixes

* Numerous exchange specific trading execution fixes
* Fix closelong / closeshort alerts for Kraken Futures and Binance Futures
* Work around Kraken cloudflare issue in ccxt library
* Add support for all new pairs on Kraken \(on other exchange this happens automatically\)
* Prevent firing orders if MM bots receive sudden strange balance or ROE values
* Fix problem that prevented proper handover from DU to RT

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

