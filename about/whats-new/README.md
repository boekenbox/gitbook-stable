---
description: >-
  Here's a quick overview of the most important changes introduced with Gunbot
  v21.9
---

# What's new?

Gunbot v21.9 introduces support for trading at Bybit, comes with a bunch of new and easy to use strategies and allows you to code your own custom strategies.

## **Upgrading**

{% hint style="success" %}
**Upgrade process using the config import tool**

It's now really easy to upload a config.js or autoconfig.json file in the interface, and import the parts you need from it. You can use this to setup a clean v21 installation while migrating your old pairs, strategies and other settings.

**Steps to start with a clean installation:** unpack v21 to a new folder and run, enter your Gunthy wallet address in the wizard and select "other" as exchange and save the changes. Then go to _Profile_ &gt; _Import settings_ and select the parts from your old config to import.
{% endhint %}

There are no breaking config changes for this version, in case you are upgrading from v18.x. or higher.

New strategy parameters are automatically merged into your strategies, as soon as you login to the browser interface.

In case you are upgrading from an older version than v18, refer to the changelogs of previous versions for instructions, or start with a fresh installation.

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

## Changes in v21

Only the most important changes are listed.

### Core / GUI

* **Bybit support:** you can now trade futures on Bybit
* **New strategies**: [spotGrid](../../trading-strategy-options/regular-strategies-spot-trading/spotgrid.md), [futuresGrid](../../trading-strategy-options/margin-trading-strategies/futures-grid.md), [spotGridAdvanced](../../trading-strategy-options/regular-strategies-spot-trading/spotgrid-advanced.md), [tenkan](../../trading-strategy-options/margin-trading-strategies/tenkan.md)
* **Custom strategies**: [code ](../../trading-strategy-options/custom-strategies.md)your own trading strategies with JavaScript
* **DCA method for futures trading**: every futures strategy can now use the Tenkan method to DCA
* **Improved profit and loss stats**: in telegram alerts and in the interface

\*\*\*\*

### Bugfixes

* Fix various order execution problems
* Fix issue in "prevent crossover" when adding pairs with AutoConfig
* Fix various GUI bugs

{% page-ref page="../../setup-and-general-settings/installation/download.md" %}

