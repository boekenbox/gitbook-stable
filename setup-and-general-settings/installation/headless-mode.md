---
description: Information about running Gunbot in headless mode.
---

# Headless mode

{% hint style="info" %}
This article is meant for power users who want to run Gunbot without using the GUI.

This is not an exhaustive overview of all available settings, just a quick overview of how to manually work with the config file. Refer to other parts of the wiki for detailed settings descriptions.
{% endhint %}

## Config file system

All Gunbot settings are defined in a single file named `config.js`. This is where you set up your exchange API keys, add pairs and define your strategies.

You can refer to the included `config-js-example.txt` file for an example of a config file with properly defined pairs and all needed parameters for adding each exchange. Throughout this wiki you'll find detailed descriptions for every parameter available in the config file.

When the config file is overwritten while Gunbot is running, the changed settings will be loaded automatically.

Make sure that no parameters are removed when setting it up. Make sure the JSON-formatting stays intact. If you are unsure about your config file, you can validate it on [https://jsonlint.com](https://jsonlint.com) \(or a similar JSON validator\).

The only actions that require using the GUI are:

* Updating master keys
* Updating the GUNTHY wallet address
* Transferring the software license to a third party

## Disabling the GUI

To disable the GUI completely, make the following change in the GUI section of `config.js`:

```text
"GUI": {
        "enabled": false,
```

## Connecting exchanges

To connect an exchange, add the relevant settings to the exchange section of `config.js`.

It looks like this:

```text
"binance": {
            "masterkey": "registered_api_key",
            "mastersecret": "secret_for_registered_api_key",
            "key": "trading_api_key",
            "secret": "secret_for_trading_api_key",
            "delay": 1,
            "type": "binance"
        },
```

Note that you can use a different API key for trading than the registered key. If you don't use a secondary key, you can just enter the registered key in the `key` parameter.

## Strategies

A strategy is defined by giving it a unique name and adding it to the `strategies` section of the config file. This strategy can then be assigned to one or more trading pairs.

It looks like this:

```text
"custom-strategy": {
            "BUY_METHOD": "tssl",
            "BUY_ENABLED": true,
            "SELL_METHOD": "tssl",
            "SELL_ENABLED": true,
            "BUY_LEVEL": 1,
            "GAIN": 0.5,

(many lines cut out to keep this page clean)

            "SL_DISABLE_BUY": false,
            "COUNT_SELL": 9999,
            "TRADES_TIMEOUT": 0
        },
```

## Defining pairs and overrides

In the `pairs` section of the config file you can add one or more pairs inside a block specifying the exchange the pairs will run on.

Each pair must be assigned an existing strategy, it must be specified if the pair is enabled or not.

```text
"Binance": {
            "BTC-LTC": {
                "strategy": "SMACROSS",
                "enabled": true,
                "override": {
                    "TRADING_LIMIT": 1000
                }
            }
        },
```

The override section allows for pair specific modifications to the assigned strategy. Any strategy parameter can be used as an override.

In the example above the pair will run the SMACROSS strategy, with a `TRADING_LIMIT` different from what is defined in the strategy itself.

