---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v23
---

# What's new?

Gunbot v23 adds support for okex v5 api, includes a native code editor, a much faster GUI and tons of bugfixes and small improvements.

## **Upgrading**

{% hint style="success" %}
**Upgrade process using the config import tool**

Using the config import tool you can easily upgrade to a clean new installation, and migrate all your settings.

**Steps to start with a clean installation:**

* unpack v22 to a new folder and run,   
* enter your Gunthy wallet address in the wizard, select "other" as exchange and save changes.   
* then go to _Profile_ &gt; _Import settings_ and select the parts from your old config to import.
{% endhint %}

There are no breaking config changes for this version, in case you are upgrading from v22.

New strategy parameters are automatically merged into your strategies, as soon as you login to the browser interface.

In case you are upgrading from an older version than v22, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

## Changes in v23

Only the most important changes are listed.

### Core / GUI changes

* **New spot strategy:** [stepGrid](https://wiki.gunthy.org/trading-strategy-options/regular-strategies-spot-trading/stepgrid) allows for almost zero settings trading
* **Exchange connectivity:** support for Okex v5 API, fix breaking change on Kucoin API. Migrate Beaxy endpoint to their new schema. _Okex and Beaxy changes require user feedback._
* **Code editor:** using the new [code editor](../../how-to-work-with-gunbot/extras/code-editor.md) you can read and edit config files, internal data and custom strategies right from the Gunbot interface. 
* **Kraken:** add support for all newly introduced markets
* **GUI performance**: much faster loading times, less resource usage, smoother charts

### **Bugfixes / Improvements \(core\)**

* Fix CEX candles
* ParseFloat leverage parameter on Bitmex
* Add postOnly orders for all limit orders at FTX
* More love to telegram bot pnl
* SpotGridAdvanced: improve handling of CT order amounts, fix a problem that prevented CT trades
* Fix an old bug that could flip a position with repeated attempts to close at ROE

### **Bugfixes / Improvements  \(GUI\)**

* Fix all custom strategy target lines
* Fix pnl not updating for new orders on gui dashboard
* Fix incorrect form background width on pairs and strategy page
* Fix issue that could crash the pairs page when removing a pair
* Fix ROE not resetting in GUI after futuresGrid closes a position
* Fix missing items for FTX futures pairs: margin ratio, unrealized pnl and maintenance margin
* Fix automatic loading of ichimoku indicator when MM\_TENKAN is used
* Fix leverage sometimes being displayed as negative number 
* Add subaccount field for ftx to gui profile page
* Rearrange chart line label positions for better chart readability
* Fix recent orders not updating automatically in overview sidebar
* Fix mobile save button not appearing
* Show potential moto entry prices on chart. 
* Fix exporting chart entities to alert module

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

