---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v21
---

# What's new?

Gunbot v21 introduces support for FTX futures, Binance coin margined futures, Txbit. Includes many bugfixes, performance improvements and better GUI stying.

## **Upgrading**

{% hint style="success" %}
**Upgrade process using the config import tool**

It's now really easy to upload a config.js or autoconfig.json file in the interface, and import the parts you need from it. You can use this to setup a clean v21 installation while migrating your old pairs, strategies and other settings.

**Steps to start with a clean installation:** unpack v21 to a new folder and run, enter your Gunthy wallet address in the wizard and select "other" as exchange and save the changes. Then go to _Profile_ &gt; _Import settings_ and select the parts from your old config to import.
{% endhint %}

There are no breaking config changes for this version, in case you are upgrading from v18.x. or higher.

New strategy parameters are automatically merged into your strategies, as soon as you login to the browser interface.

If you trade on **Binance**, you must set the appropriate [market type](../../setup-and-general-settings/profile-settings/connect-exchange.md#market-selection) in the exchange settings.

In case you are upgrading from an older version than v18, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

## Changes in v21

Only the most important changes are listed.

### Core / GUI

* **New exchanges supported**: FTX, Beaxy and Txbit
* **Coin margined futures**: now supported on Binance Futures. Set [market type: delivery](../../setup-and-general-settings/profile-settings/connect-exchange.md#market-selection)
* **Improved resource usage and cycling speed:** migrating from Node v9 to v14. Delete your gunbotgui.db file and set a new password to take full advantage
* **Improved profit and loss stats**: in telegram alerts and in the interface
* **Improved GUI styling and behavior:** stability fixes, performance improvements, and saving more state info to local storage

### Bugfixes

* Fix MM orders at Bitget
* Various GUI bugfixes and small improvements
* Automatic fetching of pair lists for Binance Futures and Kraken
* Fix problem with execution of stop limit orders on Binance Futures
* Fix problem with execution of short alerts with TradingView add-ons
* Fix 'length of undefined' error that sometimes caused a restart when trying to read the gui auth databased

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

