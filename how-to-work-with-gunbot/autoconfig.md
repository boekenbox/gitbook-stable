---
description: >-
  Automated config changes for Gunbot. Enables you to automatically add or
  remove trading pairs, or change change pair strategy overrides - using filter
  criteria you define.
---

# AutoConfig



{% hint style="success" %}
Available for Gunbot Standard and higher.
{% endhint %}

{% hint style="info" %}
Currently there is no GUI for AutoConfig. You'll need to create your own `autoconfig.json` config file, which contains the jobs it should run.

The [config marketplace](https://marketplace.gunthy.io/) has a growing number of free AutoConfig examples.

_If you are not comfortable editing config files manually, it's probably a good idea to wait until the GUI supports AutoConfig._
{% endhint %}

Gunbot AutoConfig is a collection of tools you can use to dynamically manage your config files.

You can create "jobs" to do something you would normally do by hand, for example scan an exchange for potential pairs to add, and schedule the job in a cron-like format.

Things you can currently do with AutoConfig:

* **Scan exchanges and automatically add pairs:** for example add pairs with volume &gt; 500 BTC and for which price is rising. You can even filter on buy trailing stops for all pairs on an exchange.
* **Simulate your filter settings for adding pairs:** you can collect your own data for backtesting and simulate all possible filters.
* **Scan exchanges to remove pairs from your config**: for example remove pairs without quote balance for which volume has dropped below 100 BTC.
* **Change the strategy for pairs from your config:** for example set a bag handling strategy when the pair did buy but prices dropped a lot.
* **Change the exchange delay**.
* **Monitor pair state information and automatically set pair overrides:** for example set a different `DU_BUYDOWN` after the first round of DU happened.
* **Handle hedging in bitRage**: use your own criteria to initiate hedging from BTC to USDT in bitRage \(or the other way around\).
* **Set user variables as output for a filter job**, which can be used to filter on in other jobs.

To use AutoConfig, you must have this in your `config.js` file:

```text
"AutoConfig": {
        "enabled": true
    },
```

Additionally, you must create a file named `autoconfig.json` which contains one or more jobs to process. Currently there is no GUI yet to manage this config file, but you can find detailed information on each job type in this article.

## How it works

There is a single config file for AutoConfig \(`autoconfig.json`\), which can contain one or many jobs. As soon as Gunbot \(re\)starts or the `autoconfig.json` file is changed, all jobs in the config are scheduled \(any pre-existing scheduled job is removed in this process\).

When a job is processed, changes to `config.js` are only made for pairs that have passed every filter.

* The `autoconfig.json` file can contain one or many jobs, each with its own schedule.
* One job always applies to one exchange.
* One job can have one or many filters.
* Within a job, you can set filters for which pairs it applies.

If a job successfully completes, the changes are written to `config.js` and Gunbot restarts using the new settings. If a job causes no changes, for example because it tries to place already existing overrides, it won't cause a Gunbot restart.

Schedules are set per job in a [format similar to how cron jobs are set](https://www.npmjs.com/package/node-schedule#cron-style-scheduling). If you're not used to the format, use a website like [https://crontab-generator.org/](https://crontab-generator.org/) to generate it.

## Job types \(with config examples\)

Each job type has a number of obligatory parameters, these are described below for each job type.

Additionally, there are [optional parameters](autoconfig.md#optional-parameters) you can use to extend the functionality of a job.



### Adding pairs

**Type name in config:** `addPairs`

This job type uses [ticker filters](autoconfig.md#ticker-filters).

**Job output:** add trading pairs to a specific exchange.



**Pair options:**

**include:** included pairs \(processed first\). Any pair on the exchange that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any pair on the exchange that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

**maxPairs:** maximum number of allowed pairs. In case a filter action would result in more pairs than this setting, the config will be filled up to the max number of allowed pairs. Only enabled pairs count towards maxPairs.

#### Other obligatory parameters:

**strategy:** this defines the strategy that will be assigned to pairs added by this job. It must be the exact name of an existing strategy in your `config.js` file.

**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filter types that include `Interval` in their name. 

For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. 



#### Config example

The example below shows a job that does the following:

* Scan Binance tickers every minute
* Automatically add BTC-x pairs \(but not BTC-DOGE, which is excluded\) that have a top 10 volume ranking and for which the bid/ask spread is below 0.2%
* Added pairs get the "gain" strategy assigned
* Allows for up to 25 active trading pairs on Binance

```javascript
{
    "addTopVolumePairs": {
      "pairs": {
        "exclude": "DOGE",
        "include": "BTC-",
        "maxPairs": 25,
        "exchange": "binance"
      },
      "filters": {
        "filter1": {
          "type": "maxVolumeRank",
          "max": 11 
        },
        "filter2": {
          "type": "maxSpreadPct",
          "max": 0.2 
        }
      },
      "schedule": "* * * * *",
      "snapshots": 1,
      "type": "addPairs",
      "strategy": "gain"
    }
  }
```

\*\*\*\*

### Removing pairs

**Type names in config:** `removePairs` \(uses [ticker filters](autoconfig.md#ticker-filters)\) or `removePairs2` \(uses [state filters](autoconfig.md#pair-state-filters)\).

You must have at least one pair set per exchange you use this job type on.

**Job output:** removes trading pairs from a specific exchange.



**Pair options:**

**exclude**: pairs that should not be scanned for possible removal. Any active pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled in Gunbot\) are basically the list of includes.

**noBag** \(true/false\): when true, only pairs with a balance below `MIN_VOLUME_TO_SELL` that have no open orders and are not in reversal trading, are filtered for possible removal. When set to false, all pairs in config are filtered.

**removeDisabled** \(true/false\): when true, each time a removePairs job is ran it will remove all disabled pairs for the exchange the job runs on - regardless of filter settings. Useful, for example, when you use `COUNT_SELL`.



#### Other obligatory parameters:

**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filter types that include `Interval` in their name. 

For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. 



#### Config example

The example below shows a job that does the following:

* Scan Binance tickers every ten minutes
* Remove any pair that has has a volume ranking below 20, besides pairs containing BNB or XVG in their name.
* Additionally, remove all disabled pairs from the config.

```javascript
{
  "removeCrap": {
    "pairs": {
      "exclude": "BNB,XVG",
      "noBag": false,
      "removeDisabled": true,
      "exchange": "binance"
    },
    "filters": {
      "filter1": {
        "type": "minVolumeRank",
        "min": 20
      }
    },
    "schedule": "*/10 * * * *",
    "snapshots": 1,
    "type": "removePairs"
  }
}
```

### 

### Change strategy

**Type names in config:** `changeStrategy` \(uses [ticker filters](autoconfig.md#ticker-filters)\) or `changeStrategy2` \(uses [state filters](autoconfig.md#pair-state-filters)\).

_This job type is basically the same as how removePairs works, but this one changes a pairs strategy instead of removing the pair._

You must have at least one pair set per exchange you use this job type on.

**Job output:** change assigned trading strategy for pairs on one exchange.

\*\*\*\*

**Pair options:**

**exclude**: pairs that should not be scanned for possible removal. Any active pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled in Gunbot\) are basically the list of includes.

**bag** \(true/false\): when true, only pairs with a balance below `MIN_VOLUME_TO_SELL` that have no open orders and are not in reversal trading, are filtered for possible removal. When set to false, all pairs in config are filtered. 

\*\*\*\*

**Other obligatory parameters:**

**strategy:** the target strategy to set for pairs matching all filters.

**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filter types that include `Interval` in their name. 

For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. 



#### Config example

The example below shows a job that does the following:

* Scan Binance tickers every 15 minutes
* Change the assigned strategy for any pair that has below median trading volume, except BTC-LTC
* Assign the strategy "baghandler" to these pairs

```javascript
{
    "changeStrat": {
        "pairs": {
            "exclude": "BNB-LTC",
            "bag": true,
            "exchange": "binance"
        },
        "filters": {
            "filter1": {
                "type": "belowMedianVolume"
            }
        },
        "schedule": "*/15 * * * *",
        "snapshots": 1,
        "type": "changeStrategy",
        "strategy": "baghandler"
    }
}
```

### 

### Managing overrides

**Type name in config:** `manageOverrides`

This job type uses [state filters.](autoconfig.md#pair-state-filters)

**Job output:** changes overrides for pairs on a specific exchange.



**Pair options:**

**include:** included pairs \(processed first\), can not be empty. Any active trading pair that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any active trading pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

\*\*\*\*

**Other obligatory parameters:**

**overrides**: contain one or more overrides, these will be set for each pair that passes all filters when a job is executed.

**clearOverrides** \(true/false\): when set to true, all existing overrides for a pair are removed before placing the new ones.

\*\*\*\*

#### Config example

The example below shows a job that does the following:

* Scan Binance pairs every minute, process filters on all active trading pairs that include "USDT" or "BNB" and do not include "DOGE" or "ETH" in their pair name.
* Set a `DU_BUYDOWN` override for all pairs that have `ducount` 1

```javascript
{
    "DynamicDU1": {
        "pairs": {
            "exclude": "DOGE,ETH",
            "include": "USDT,BNB",
            "exchange": "binance"
        },
        "filters": {
            "ducount": {
                "type": "exact",
                "ducount": 1
            }
        },
        "overrides": {
            "DU_BUYDOWN": 3
        },
        "clearOverrides": false,
        "schedule": "* * * * *",
        "type": "manageOverrides"
  }
}
```



### Change exchange delay

**Type name in config:** `changeDelay`

This job type uses [state filters.](autoconfig.md#pair-state-filters)

**Job output:** changes the exchange delay for a specific exchange.



**Pair options:**

**include:** included pairs \(processed first\), can not be empty. Any active trading pair that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any active trading pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

\*\*\*\*

**Other obligatory parameters:**

**delay**: exchange delay in seconds, this value will be set when one or more pairs in the job pass all filters.

\*\*\*\*

#### Config example

The example below shows a job that does the following:

* Scan Binance pairs every minute, process filters on all active trading pairs that include "USDT" or "BNB" and do not include "DOGE" or "ETH" in their pair name.
* Set a the exchange delay for Binance to 10 in case at least one pair matches the filter

```javascript
{
    "DynamicDU1": {
        "pairs": {
            "exclude": "DOGE,ETH",
            "include": "USDT,BNB",
            "exchange": "binance"
        },
        "filters": {
            "ducount": {
                "type": "exact",
                "ducount": 1
            }
        },
        "delay": 10,
        "schedule": "* * * * *",
        "type": "changeDelay"
  }
}
```



### Manage bot settings

**Type names in config:** `manageBotSettings` \(uses [state filters](autoconfig.md#pair-state-filters)\).

_This job type is basically the same as how changeStrategy works, but this type is able to change the global bot settings._

You must have at least one pair set per exchange you use this job type on. 

**Job output:** change assigned parameters in the bot section. Happens when at least one pair passes all filters

\*\*\*\*

**Pair options:**

**exclude**: pairs that should not be filtered. Any active pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled in Gunbot\) are basically the list of includes.

\*\*\*\*

**Other obligatory parameters:**

**bot:** the target bot settings.

\*\*\*\*

#### Config example

```javascript
{
"botsettings": {
"pairs": {
"exclude": "",
"include": "BTC-",
"exchange": "binance"
},
"filters": {
"price": {
"type": "biggerThan",
"Ask": 0.00000001
}
},
"bot": {
"BOT_DELAY": 20,
"BOT_CCLEAN": 9991
},
"schedule": "*   * *",
"type": "manageBotSettings"
}
}
```



### Hedge \(for Gunbot\)

**Type names in config:** `hedgeGB` \(uses [ticker filters](autoconfig.md#ticker-filters)\) or `hedgeGB2` \(uses [state filters](autoconfig.md#pair-state-filters)\).

You must have at least one pair set per exchange you use this job type on.



**Job output:** 

You can define a "hedge pair" like USDT-BTC, between which Gunbot should hedge when the market conditions are right for it. If you prefer hedging between completely other currrencies, like BTC-ETH - you can do that too.

It works with two autoconfig jobs, in which you define when it should hedge and to which currency it should hedge.

* When a pair can be hedged without intermediate pair, autoconfig will change a pair like BTC-ETH to USDT-ETH and panic sell directly to ETH.
* When an intermediate pair is needed, the pair will be panic sold first and then traded on the defined hedge pair.

\*\*\*\*

**Pair options:**

Pair filtering is hardcoded to the defined hedge pair.

**exclude**: used as placeholder

**bag:** used as placeholder

**baseFrom:** base currency to hedge away from

**baseTo:** base currency to hedge to

**hedgePair:** pair used for hedging, like USDT-BTC

\*\*\*\*

**Other obligatory parameters:**

**setVariable:** hedging will only work when both hedging jobs set a variable with the currency it last hedged to, this makes sure that the direction can be tracked for the next hedging action.



#### Config example

You'll need two jobs to do this, one for each hedging direction:

```javascript
{
"hedging": {
"pairs": {
"exclude": "",
"exchange": "binance",
"bag": false,
"baseFrom": "BTC",
"baseTo": "USDT",
"hedgePair": "USDT-BTC"
},
"filters": {
"filter1": {
"type": "yourTickerFilter",
"min": "0.0000000001"
},
"filter2": {
"type": "variableNotExist",
"hegdedTo": "BTC"
}
},
"filters2": {
"filter1": {
"type": "yourTickerFilter",
"min": "0.0000000001"
},
"filter2": {
"type": "variableExact",
"hegdedTo": "BTC"
}
},
"setVariable": {
"hegdedTo": "USDT"
},
"strategy": "gain",
"schedule": "1 /4  * *",
"type": "hedgeGB",
"debug": false
}
}
```

The second job would be a mirror job, with all hedge currencies reversed.

Two important notes:

* keep the hedge pair in your config at all times, prefably with buy/sell disabled to avoid crossover between base/quote.
* set the filter for "type": "variableNotExist", ONLY in the job that will take care of the next hedging action. For example, when you start out with BTC-x pairs only, add this filter to the job that hedges to USDT.

### 

### Hedge \(for bitRage\)

**Type name in config:** `hedge`

This job type uses [state filters.](autoconfig.md#pair-state-filters)

**Job output:** initiates hedging in bitRage.



**Pair options:**

**include:** included pairs \(processed first\), can not be empty. Any active trading pair that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any active trading pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

\*\*\*\*

**Other obligatory parameters:**

**hedgeTo** \(USDT/BTC\): defines which currency to start hedging to.

**brStrat**: defines which strategy is used for bitRage. 

\*\*\*\*

#### Config example

The example below shows a job that does the following:

* Scan for a user variable, every minutes
* Once this variable is found, it places filteredBase, filteredQuote and filteredPair blocks as defined in the job, for kucoin \(this part is optional\)
* Once this variable is found, it makes the following changes in the strategy `bitrageStrat`, it sets/changes the following parameters so that hedging will start:

```text
"AUTOSELL": true,
"MAIN_BASE": "USDT",
"BR_PANIC_SELL": true,
```

```javascript
{
    "Hedge_to_USDT": {
		"pairs": {
			"exclude": "",
			"include": "",
			"exchange": "kucoin",
			"filteredBase": [
				"TUSD",
				"PAX",
				"KCS",
				"NUSD",
				"ETH",
				"DAI",
				"USDC",
				"TRX",
				"NEO",
				"BTC"
			],
			"filteredQuote": [
				"GGC"
			],
			"filteredPair": [
				"BTC-GGC",
				"USDT-GGC",
				"BTC-KCS",
				"USDT-KCS",
				"KCS-XRP",
				"KCS-EOS",
				"KCS-LTC",
				"KCS-NEO",
				"KCS-MTV",
				"KCS-GO",
				"KCS-CS",
				"ETH-KCS",
				"NEO-EOS",
				"DAI-BTC",
				"DAI-ETH",
				"DAI-MKR",
				"DAI-USDT"
			]
		},
		"filters": {
			"variable": {
				"type": "variableExact",
				"readyToHedge": "USDT"
			}
		},
		"setVariable": {
			"hedgingStarted": true
		},
		"schedule": "* * * * *",
		"type": "hedge",
		"hedgeTo": "USDT",
		"brStrat": "bitrageStrat"
	},
}
```

### 

### Filtered quote \(for bitRage\)

**Type name in config:** filteredQuote

This job type uses [ticker filters](autoconfig.md#ticker-filters).

**Job output:** It adds the quote of each pair passing all filters to the filteredQuote list. Each time the job runs, the list gets completely replaced with the new results.



**Pair options:**

**include:** included pairs \(processed first\). Any pair on the exchange that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any pair on the exchange that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.



#### Config example

The example below would do the following:

* scan all binance BTC-x pairs
* find pairs with a bid/ask spread higher than 0.7%
* save the quote coins from these pairs in the filteredQuote list.

```javascript
{
"filteredQuote": {
"pairs": {
"include": "BTC-",
"exclude": "",
"exchange": "binance"
},
"filters": {
"spread": {
"type": "minSpreadPct",
"min": 0.7
},
"schedule": "* * * * *",
"type": "filteredQuote",
"debug": false
}
}
```

\*\*\*\*

### Optional parameters

Jobs can be extended with additional parameters, some work in all job types, some are specific to certain job types.



#### Optional parameters for all job types:

**enabled** \(true/false\): When false, the job is disabled and won't be executed.

**debug** \(true/false\): When true, the job generates detailed logs in the console for each filter.

[settings for user variables](autoconfig.md#user-variables)

**brStrat**: defines which strategy is used for bitRage. Only needed if you use Autoconfig to automate hedging for bitRage.



#### Optional parameters for jobs using ticker filters:

**resume** \(true/false\): when true, saved ticker snapshots are loaded from file after Gunbot restarts. When false, history needs to be built up again after restarting.

**history**: defines how many ticker entries should be kept in [history ](autoconfig.md#ticker-history-filters)storage.

**historyInterval:** defines the minimum interval in minutes between history entries.

\*\*\*\*

#### **Optional parameter for `addPairs` and `manageOverrides`**

**setITB** \(true/false\): When true, each pair matching all filters will get an additional `IGNORE_TRADES_BEFORE` override, the value being the timestamp for the time the job ran.

**delay** \(time in seconds\): When set, the exchange delay gets adjusted to the specified value when at least one pair passes all filters. For manageOverrides only.



#### Optional parameters for `addPairs`

**overrides**: this job type can also add overrides when it adds new pairs. To do so, add a section with overrides to the job, just like you would in a `manageOverrides` job.

**Bitrage filters:** when used for Bitrage, you can have an addPairs job replace the contents of the exchange filter settings. To do so, add the filters in the pair section of the job as shown below:

**noCrossOver** \(true/false\): needs to be put in the pairs section. When set, Autoconfig will never add two pairs with different Base but the same Quote currency.

```javascript
"pairs": {
            "exclude": "",
            "include": "BTC-",
            "maxPairs": 10,
            "exchange": "kucoin",
            "filteredQuote": ["DOGE"],
      "filteredPair": ["BTC-DOGE"],
      "filteredBase": ["BTC","ETH","USDS","TUSD","USDC","PAX","XRP","TRX","BUSD","NGN"]
        },
```



#### Optional parameters for `removePairs`

**removeBase**: can be used to remove all pairs from a specified base currency that use the bitRage strategy specified in `brStrat`. This is useful to clean out pairs from a specific base after hedging.

**notRemoveBefore** \(in pairs section\): value in minutes. Don't remove pairs from config if it's not in the config longer than specified.



#### Optional parameters for `hedge`

**Bitrage filters:** when used for Bitrage, you can have an addPairs job replace the contents of the exchange filter settings. To do so, add the filters in the pair section, as shown above.

## Filter options

### Ticker filters

**For job types:** `addPairs`, `removePairs`, `changeStrategy`

Ticker filters use data collected from the exchange tickers, at the moment the job runs.

You can use the following filter types for adding and removing pairs. Please note that not every filter type works on every exchange, due to the fact that some exchanges don't offer the required data. On Huobi AutoConfig uses "last" price instead of bid/ask for all filters that work with prices.

![Data availability for certified spot exchanges. Ticker filters are not available for Bitmex.](../.gitbook/assets/image%20%2828%29.png)

_Filters for prices use ask when adding pairs and bid when filtering for removal or changing strategy._

\_\_

<table>
  <thead>
    <tr>
      <th style="text-align:left"><em>Type</em>
      </th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Extras</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>minPrice</code>
      </td>
      <td style="text-align:left">Filter returns true when price is higher than set.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxPrice</code>
      </td>
      <td style="text-align:left">Filter returns true when price is lower than set.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minPricePctChangeInterval</code>
      </td>
      <td style="text-align:left">Filter returns true if the current price is at least x% higher than the
        average price of all snapshots.</td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxPricePctChangeInterval</code>
      </td>
      <td style="text-align:left">Filter returns true if the current price is at least x% lower than the
        average price of all snapshots.</td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minVolumePctChangeInterval</code>
      </td>
      <td style="text-align:left">Filter returns true if the current 24h volume is at least x% higher than
        the average 24h volume of all snapshots.</td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxVolumePctChangeInterval</code>
      </td>
      <td style="text-align:left">Filter returns true if the current 24h volume is at least x% lower than
        the average 24h volume of all snapshots.</td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minVolume24h</code>
      </td>
      <td style="text-align:left">Filter returns true if 24h volume is higher than set, volume in base.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxVolume24h</code>
      </td>
      <td style="text-align:left">Filter returns true if 24h volume is lower than set, volume in base.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minVolatilityPct24h</code>
      </td>
      <td style="text-align:left">Filter returns true if 24h price percentage change is higher than set.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxVolatilityPct24h</code>
      </td>
      <td style="text-align:left">Filter returns true if 24h price percentage change is lower than set.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minSpreadPct</code>
      </td>
      <td style="text-align:left">Filter returns true if percentage difference between bid and ask is higher
        than set.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxSpreadPct</code>
      </td>
      <td style="text-align:left">Filter returns true if percentage difference between bid and ask is lower
        than set.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minSlopePctInterval</code>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if the <a href="https://tulipindicators.org/linregslope">slope</a> for
          all prices in snapshots is bigger than set.</p>
        <p></p>
        <p>Slope is expressed as a percentage of the last price. A slope of 1 means
          that, according to a simple linear regression, the next collected ticker
          price is likely to be 1% higher than the last one.</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxSlopePctInterval</code>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if the <a href="https://tulipindicators.org/linregslope">slope</a> for
          all prices in snapshots is smaller than set.</p>
        <p></p>
        <p>Slope is expressed as a percentage of the last price. A slope of 1 means
          that, according to a simple linear regression, the next collected ticker
          price is likely to be 1% higher than the last one.</p>
        <p></p>
        <p>Only executed when max snapshot sample size is reached.</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minStandardDevPctInterval</code>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if the <a href="https://tulipindicators.org/stddev">Standard Deviation</a> for
          all prices in snapshots is bigger than set.</p>
        <p></p>
        <p>Standard Deviation is expressed as a percentage of the last price.</p>
        <p></p>
        <p>Only executed when max snapshot sample size is reached.</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p><code>maxStandardDevPctInterval</code>
        </p>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if the <a href="https://tulipindicators.org/stddev">Standard Deviation</a> for
          all prices in snapshots is smaller than set.</p>
        <p></p>
        <p>Standard Deviation is expressed as a percentage of the last price.</p>
        <p></p>
        <p>Only executed when max snapshot sample size is reached.</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>belowMedianVolume</code>
      </td>
      <td style="text-align:left">Filter returns true if the base volume for a pair is lower than the median
        base volume for all pairs with the same base currency on the exchange.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>aboveMedianVolume</code>
      </td>
      <td style="text-align:left">Filter returns true if the base volume for a pair is higher than the median
        base volume for all pairs with the same base currency on the exchange.</td>
      <td
      style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>minVolumeRank</code>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if a pairs 24h volume rank (rankings are specific
          per base) is higher than set.</p>
        <p></p>
        <p>This filter is only useful if you want to exclude some of the top ranking
          volume pairs, for example set it to 5 to only allow pairs that have a volume
          rank of 6 or higher.</p>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>maxVolumeRank</code>
      </td>
      <td style="text-align:left">
        <p>Filter returns true if a pairs 24h volume rank (rankings are specific
          per base) is lower than set.</p>
        <p></p>
        <p>Setting it to 10, for example, would only include pairs that have a top10
          volume ranking.</p>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>bullishStandardDeviationChannel</code>
      </td>
      <td style="text-align:left">
        <p>It works similar to described here: <a href="http://www.forexpromos.com/what-is-standard-deviation-channel">http://www.forexpromos.com/what-is-standard-deviation-channel</a>
        </p>
        <p></p>
        <p>The filter passes when:</p>
        <ul>
          <li>slopePct is positive</li>
          <li>max snapshot count is reached</li>
          <li>price is within a defined range from the lower band</li>
        </ul>
        <p>(it would detect a downwards breakout from an upwards channel)</p>
        <p></p>
        <p>Range 0 = same price as lower band</p>
        <p>Range 100 = same price as upper band</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>bearishStandardDeviationChannel</code>
      </td>
      <td style="text-align:left">
        <p>It works similar to described here: <a href="http://www.forexpromos.com/what-is-standard-deviation-channel">http://www.forexpromos.com/what-is-standard-deviation-channel</a>
        </p>
        <p></p>
        <p>The filter passes when:</p>
        <ul>
          <li>slopePct is negative</li>
          <li>max snapshot count is reached</li>
          <li>price is within a defined range from the upper band</li>
        </ul>
        <p>(it would detect an upwards breakout from a downwards channel)</p>
        <p></p>
        <p>Range 0 = same price as upper band</p>
        <p>Range 100 = same price as lower band</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>linearRegressionChannel</code>
      </td>
      <td style="text-align:left">
        <p>Same as &quot;Linear regression&quot; indicator on TradingView. Standard
          deviation is hardcoded to 2.</p>
        <p></p>
        <p>The filter passes when:</p>
        <ul>
          <li>max snapshot count is reached</li>
          <li>price is within a defined range from the lower band</li>
        </ul>
        <p>Range 0 = same price as lower band</p>
        <p>Range 100 = same price as upper band</p>
      </td>
      <td style="text-align:left"><code>lastSnapshots</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>allowsHedging</code>
      </td>
      <td style="text-align:left">Filter returns true if a pair can be hedged to another base currency without
        using an intermediate pair.</td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>buyTrailing</code>
      </td>
      <td style="text-align:left"><a href="https://wiki.gunthy.org/how-to-work-with-gunbot/autoconfig#trailing-filters">See details</a>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>volumeTrailing</code>
      </td>
      <td style="text-align:left"><a href="https://wiki.gunthy.org/how-to-work-with-gunbot/autoconfig#trailing-filters">See details</a>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>slopeTrailing</code>
      </td>
      <td style="text-align:left"><a href="https://wiki.gunthy.org/how-to-work-with-gunbot/autoconfig#trailing-filters">See details</a>
      </td>
      <td style="text-align:left">n/a</td>
    </tr>
  </tbody>
</table>\_\_

The extra input called `lastSnapshots` lets you use only the last x snapshots to calculate the filter. 

If for example your job collects 100 snapshots but you want a specific filter to only use the last 10 snapshots to calculate slope, you can do that.

**Usage examples:**

```javascript
"maxStandardDevPctInterval": {
  "type": "minVolumePctChangeInterval",
   "max": 1,
   "lastSnapshots": 3
}

"hedge": {
"type": "allowsHedging",
"BTC": "USDT"
}
```



Config building blocks for ticker filters:

```text
"strat": {
    "type": "minSpreadPct",
    "min": 0.5
},
"volume": {
    "type": "belowMedianVolume"
},
"stdev": {
    "type": "maxStandardDevPctInterval",
    "max": 0.3
},
"channel": {
		"type": "bullishStandardDeviationChannel",
		"range": -10
}
```

\_\_

\_\_

#### Trailing filters

The filter types `buyTrailing` , `volumeTrailing` and `slopeTrailing` are ticker filters that trails down prices or volume very [similar](basic-workings/trailing.md#buy-trailing) to a regular Gunbot strategy with buy trailing, you can use it to add pairs to your config only after they have hit their trailing stop. Useful for trailing massive numbers of pairs without the downsides of long cycling times.

_These filter types can only be used in `addPairs` jobs on exchanges that provide ask prices or volume in tickers, only works when used in the first filter set of a job._

**Config example for buy trailing**

The example below will:

* Collect up to 60 ticker snapshots, adds a new snapshot every time the job runs \(every minute\)
* Uses the 60 collected bid prices for a pair to calculate an EMA
* Continuously trail down all pairs, using a `trailingRange` of 1% of the ask price
* The filter passes when the ask prices crosses over the trailing stop, while being below `buyLevel` \(which is a percentage below the EMA calculated by this filter\)

Volume trailing works exactly as above, the only difference is that base volume is used where prices are used in buy trailing.

Slope trailing works just like buy trailing, the only difference is that the slope percentage of a pair is trailed. Buy level is based on the same EMA as it is for buy trailing.

```javascript
{
	"trailingExample": {
		"pairs": {
			"exclude": "",
			"include": "BTC-,USDT-",
			"maxPairs": 10,
			"exchange": "binance"
		},
		"filters": {
			"trailing": {
				"type": "buyTrailing",
				"buyLevel": 0.5,
				"trailingRange": 1
			}
		},
		"schedule": "* * * * *",
		"type": "addPairs",
		"strategy": "instantBuy",
		"enabled": true,
		"resume": true,
		"snapshots": 60
	},
```

\_\_

#### Ticker history filters

Most ticker filters are also available as `*History` variant. These work in the same way as described above, but they use a different data set as input. Available history filters:

* `minPriceHistory`
* `maxPriceHistory`
* `maxVolumeRankHistory`
* `minVolumeRankHistory`
* `minPricePctChangeIntervalHistory`
* `maxPricePctChangeIntervalHistory`
* `minVolumePctChangeIntervalHistory`
* `maxVolumePctChangeIntervalHistory`
* `minVolume24hHistory`
* `maxVolume24hHistory`
* `minVolatilityPct24hHistory`
* `maxVolatilityPct24hHistory`
* `minSpreadPctHistory`
* `maxSpreadPctHistory`
* `minSlopePctIntervalHistory`
* `maxSlopePctIntervalHistory`
* `minStandardDevPctIntervalHistory`
* `maxStandardDevPctIntervalHistory`
* `bearishStandardDeviationChannelHistory`
* `bullishStandardDeviationChannelHistory`

History filters take one additional input, defining which history data should be used. the config for a history filter looks like:

```text
"filter": {
				"type": "minPriceHistory",
				"min": 10,
				"historySource": 6
			}
```

The `historySource` parameter in the example above means that it will use the price of the history entry with number 6. The oldest history entry has number 0.

How the history is built up is defined by the following parameters in the root level:

```text
"history": 7,
"historyInterval": 15,
```

The example above would collect 7 history entries, with a minimum interval of 15 minutes.

New history entries are saved as follows:

* When there is no history, the oldest ticker snapshot will be added as first history entry.
* When there is at least one history entry, a new one is added when the time difference between the oldest ticker snapshot and the latest history entry is bigger than the time defined in `historyInterval`. The oldest ticker snapshot will be added as newest history entry. In case the maximum number of history entries is reached, the oldest history entry will get deleted once a new one gets added.

The config example below shows a job that filters for:

* BTC pairs that currently ranks top10 for 24h volume
* The 5 history entries have a slope of at least 1%
* Pair must have ranked top10 24h volume in the oldest history entry

```text
If oldest snapshot is > 60 minutes older than newest history entry, 
it gets moved to history

                                                Snapshots, 1m interval 
                                                [s]  [s]  [s]  [s]  [s]  
History entries, 60m interval                    |  
[0]  [1]  [2]  [3]  [4]                          |
                         ^-----------------------*


"addMoon": {
		"pairs": {
			"exclude": "",
			"include": "BTC-",
			"maxPairs": 10,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "maxVolumeRank",
				"max": 10
			},
			"filter2": {
				"type": "minSlopePctIntervalHistory",
				"min": 1,
				"historySource": 4
			},
			"filter3": {
				"type": "maxVolumeRankHistory",
				"max": 10,
				"historySource": 0
			}
		},
		"schedule": "* * * * * *",
		"type": "addPairs",
		"strategy": "moon",
		"snapshots": 5,
		"history": 5,
		"historyInterval": 60,
		"resume": true
	},
```

{% hint style="info" %}
Both snapshots and history cause a relatively high load on I/O operations. Depending on your system, allowing for too many entries saved can negatively impact performance.
{% endhint %}



### Pair state filters

For job types: `manageOverrides`, `changeDelay`, `removePairs2`, `changeStrategy2`

State filters use data from the internal ledger in Gunbot, which has all data for pairs that already cycled since Gunbot last \(re\)started.

![Available pair state filters](../.gitbook/assets/image%20%2861%29.png)

Formula used in `differenceBigger`:  
`100 * ((ema2 - ema1) / ema1) > delta`

Formula used in `differenceSmaller`:  
`100 * ((ema2 - ema1) / ema1) < delta`

_Formula examples use ema1 and ema2 like set in the screenshot above. Of course you can compare any two keys. The position of the keys to compare in the config file do matter._

\_\_

### Generic filters

Generic filters can be used in any job type, regardless if they primarily use ticker or state filters.

| _Type_ | Description |
| :--- | :--- |
| `variableExact` | Filter returns true when variable value is exactly as defined. |
| `variableNotExist` | Filter returns true when variable key does not \(yet\) exist. |
| `pairVariableExact` | Filter returns true when pair specific variable is exactly as defined. Can use an extra "exchange" input to check pair variables for another exchange. |
| `strategyName` | Filter returns true when strategy of an enabled pair is like defined. |
| `minTimeInConfig` | Filter returns true when pair is longer in config than a set value in minutes. |
| `maxTimeInConfig` | Filter returns true when pair is not longer in config than a set value in minutes. |
| `maxSameOrder` | Filter returns true when last x orders for different pairs do NOT have the same defined type |
| `minTimeSinceOrder` | Filter returns true when at least x minutes have passed since a defined order type, for the same pair. |
| `maxTimeSinceOrder` | Filter returns true when at most x minutes have passed since a defined order type, for the same pair. |

Config building blocks for generic filters:

```text
"timeSince": {
"type": "maxTimeSinceOrder",
"min": 30,
"trigger": "StopLimit"
}

Usage:
"change": {
"type": "minPricePctChangeLastOrder",
"delta": 1,
"trigger": "StrategyBuy"
}

"time": {
"type": "minTimeInConfig",
"min": 60
},

"time": {
"type": "maxTimeInConfig",
"max": 90
} 

"var": {
"type": "variableNotExist",
"hedge": "BTC"
}

"strat": {
"type": "strategyName",
"name": "moon"
}

"sameOrders": {
"type": "maxSameOrder",
"lastOrders": 4,
"trigger": "StrategyBuy"
}
```

 **Available trigger types for filters using triggers.** 

Note: only the most recent order for each pair is available for analysis, the orders must be placed by a Gunbot strategy. Data is kept between restarts, also for pairs that are no longer actively traded with Gunbot.

* StrategyBuy
* StrategySell
* StopLimit
* Close
* RTSell
* RTBuy
* RTBuyback
* DCA
* Cancelled

### User Variables

Each job can set one or more user defined variables, which can be used to filter on in other jobs.

You can use both global variables, and pair specific variables.

This allows for more complex, but also easier to handle, filter setups, because:

* Jobs can depend on another.   
* you don't need to repeat multiple filter conditions across multiple jobs. 

You can have one job monitoring a specific condition \(for example the distance between price and liquidation price\) and set a variable like `"liquidationStop": true` in case its conditions happen. Other jobs that depend on this liquidation stop then only have to set one filter looking for an exact match for `"liquidationStop": true` instead of needing to repeat the same filters set in the job that monitors the the distance between price and liquidation price.

A job sets a variable when:

* At least one pair passed all filters for this job.
* `setVariable` is defined in the jobs config.

`setVariable` looks like this:

```text
"setVariable": {
            "userVariable1": true
        },
```

It can contain one or more variables, their value can be filtered as exact match only. Besides true/false, you could also set number values, or strings.

To use pair specific variables, use a block like this:

```text
"setPairVariable": {
"awesomeVariable": true,
"evenBetter": true
},
```

In case you set a variable that was previously set with a different value, the new value will overwrite the old one. Setting one variable has no effect on other possible variables that are already set.

All variables are written to file and are imported anytime Gunbot restarts. Please be aware that this won't always work, file corruption can happen - for example when a write action is happening right in the moment that Gunbot is closed. It's a good idea to not fully depend on saved variables, and run the jobs that set them relatively frequently.

To read a variable, use the filter type `variableExact`. It can be used in all job types.

```text
"filter": {
"type": "variableExact",
"userVariable1": true
}
```

This filter type will return true when `userVariable1` has a value of `true`.

Pair variables can be read with a filter like this:

```text
"pairVar": {
"type": "pairVariableExact",
"awesomeVariable": true
}
```

{% hint style="info" %}
Variables are entirely optional. It's no problem when no `setVariable` exists in a job.
{% endhint %}

The same job that sets a variable, can also **reset** them in case no pairs passed all filters. This option is called `resetVariable`.

`resetVariable` looks like this:

```text
"resetVariable": {
            "userVariable1": true
},
```

It can contain one or more variables, their value can be filtered as exact match only. Besides true/false, you could also set number values, or strings.

To reset pair specific variables, use a block like this:

```text
"resetPairVariable": {
"awesomeVariable": false,
"evenBetter": false
},
```

### Multiple filter sets

Instead of using a single set of filters, you can also add multiple sets of filters in a job.

Use this when you want to monitor for different conditions in a single job, if a pair passes all filters in any of the filter sets, changes are made.

Besides the obligatory first set of filters, you can add up to 9 more sets. named `filters2` to `filters10`

**Config example:**

```text
{
	"example": {
		"pairs": {
			"exclude": "",
			"include": "",
			"maxPairs": 500,
			"noBag": false,
			"exchange": "binance"
		},
		"filters": {
			"price": {
				"type": "minPrice",
				"min": 0.0000001
			}
		},
		"filters2": {
			"minVolume24h": {
				"type": "minVolume24h",
				"min": 100
			}
		},
		"schedule": "*/30 * * * * *",
		"type": "removePairs",
		"enabled": true
	}
}
```



## Backtesting for addPairs jobs

Results for addPairs jobs can be simulated using ticker data you've collected yourself. Self-collection is needed because the type of historic data AutoConfig uses is not publicly available.

Before you can backtest, you need to collect a dataset with a "collectData" job. This is a simple autoconfig job that collects and saves ticker data according to the job schedule and exchange.

```text
"30-secs": { 
    "pairs": { 
        "exchange": "binance" 
    }, 
    "schedule": "/30 * * * * *", 
    "type": "collectData", 
    "snapshots": 1000,
    "enabled": true, 
    "debug": false 
}
```

The schedule in this job should be the same as the schedule you'd use in an `addPairs` job. It will collect up to 5 ticker snapshots and removes older snapshots in case they exist. 

You can use any number of snapshots in this job type. If you omit the snapshots parameter, it will collect ticker snapshots for as long as your system has free disk space.

Ticker data for this collection job is saved in /30-secs-tickers \(\#jobname\#-tickers\). Do make sure you never save any other files in this folder.

As long as the `collectData` job runs, it keeps collecting tickers. It is not recommended to permanently collect tickers while also running other autoconfig jobs that use tickers: API usage weight is relatively high, and it will slow down Gunbot performance slightly.

To backtest using the collected data, run a job with type `backtesting`. Do this while you have Gunbot core and other autoconfig jobs disabled, because it's very cpu intensive and you don't want other jobs to interrupt.

A backtesting job is basically the same as an addPairs job, you can use all available filter / filterset options. Collected ticker data is replayed as if it were live data, the results should 100% match the results from a regular addPairs job. Results are logged the same way as regular jobs, the debug option for jobs is also available.

Results are saved to a .csv file that lists when pairs would have been added, and what the ask price was at that moment.

Backtesting jobs also use the autoconfig scheduler, although it's kind of pointless. It's recommended to set a silly schedule that will almost never execute and use the `"onStart": true` option so that the job runs as soon as it gets enabled.

Backtesting does not take `maxPairs` into consideration \(and will not in the future\), results are purely based on the filters you use.

Job example for backtesting:

```javascript
"30-secs": {
		"pairs": {
			"exclude": "",
			"include": "BTC-",
			"exchange": "binance"
		},
		"filters": {
			"slope": {
				"type": "minSlopePctInterval",
				"min": 0.02
			},
			"volume": {
				"type": "minVolume24h",
				"min": 75
			},
			"pricehistory0": {
				"type": "maxPricePctChangeIntervalHistory",
				"max": 5,
				"historySource": 9
			}
		},
		"schedule": "1 1 1 1 1",
		"type": "backtesting",
		"tickersFolder": "30-secs-tickers",
		"enabled": true,
		"onStart": true,
		"history": 10,
		"historyInterval": 2,
		"debug": false,
		"snapshots": 10
	}
```

The data source for backtesting is set with the `"tickersFolder"` parameter. Set it to the exact folder name where your collected ticker files are. i

## Various

* It's fine to schedule many jobs for the same times, but in case multiple of those jobs causes a config change, the first one to finish will write it's changes and the others jobs will need to wait for another chance.
* The output of every job is that the `config.js` file is updated, this will always cause Gunbot to restart.
* Each job can contain an optional "`debug": true` line, when set the job will show detailed console log output for the filters in this job. Only use this in the moment you really need it, it can slow down Gunbot performance significantly due to the large amount of data that needs to be logged to the console.
* Things that make AutoConfig crash:  change the exchange for a ticker filtering job while having `"resume": true`, using a non-existent key in a state filter, etc.
* Data is read from either exchange tickers or the internal Gunbot memory with pair state info. To find out which pair state data to filter on is available, look in the pairs state file in the Gunbot `/json` folder.
* You can disable telegram notifications per job, just include this line in your job: `"muteTG": true`
* Almost every key/value in pair state files can be filtered, as long as they are on the first level \(not inside additional arrays or objects\)

![First level elements like these can be used for filtering.](../.gitbook/assets/image%20%2864%29.png)

![Elements like these cannot be used for filtering.](../.gitbook/assets/image%20%288%29.png)

## Example config with many job types and filters

You don't want to use this ever in this form, but use it as reference for how each job can be formatted.

```javascript
{
	"addPairs-jobname": {
		"pairs": {
			"exclude": "DOGE,XLM,PAX",
			"include": "BTC,USDT",
			"maxPairs": 25,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "minPrice",
				"min": 0.0000001
			},
			"filter2": {
				"type": "maxPrice",
				"max": 0.0000010
			},
			"filter3": {
				"type": "minPricePctChangeInterval",
				"min": 0.00002
			},
			"filter4": {
				"type": "maxPricePctChangeInterval",
				"max": 1
			},
			"filter5": {
				"type": "minVolumePctChangeInterval",
				"min": 10
			},
			"filter6": {
				"type": "maxVolumePctChangeInterval",
				"max": 50
			},
			"filter7": {
				"type": "minVolume24h",
				"min": 500
			},
			"filter8": {
				"type": "maxVolume24h",
				"max": 1000
			},
			"filter9": {
				"type": "minVolatilityPct24h",
				"min": 1
			},
			"filter10": {
				"type": "maxVolatilityPct24h",
				"max": 1
			},
			"filter11": {
				"type": "minSpreadPct",
				"min": 0.00001
			},
			"filter12": {
				"type": "maxSpreadPct",
				"max": 1
			},
			"filter13": {
				"type": "minSlopePctInterval",
				"min": 0.00001
			},
			"filter14": {
				"type": "maxSlopePctInterval",
				"max": 1
			},
			"filter17": {
				"type": "belowMedianVolume"
			},
			"filter18": {
				"type": "aboveMedianVolume"
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			}
		},
		"schedule": "* * * * *",
		"type": "addPairs",
		"strategy": "gain",
		"snapshots": 2,
		"resume": false,
		"debug": "true",
		"setVariable": {
			"userVariable1": true
		},
		"enabled": true
	},
	"removePairs-jobname": {
		"pairs": {
			"exclude": "BNB,XVG",
			"noBag": false,
			"removeDisabled": true,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "minPrice",
				"min": 0.0000001
			},
			"filter2": {
				"type": "maxPrice",
				"max": 0.0000010
			},
			"filter3": {
				"type": "minPricePctChangeInterval",
				"min": 0.00002
			},
			"filter4": {
				"type": "maxPricePctChangeInterval",
				"max": 1
			},
			"filter5": {
				"type": "minVolumePctChangeInterval",
				"min": 10
			},
			"filter6": {
				"type": "maxVolumePctChangeInterval",
				"max": 50
			},
			"filter7": {
				"type": "minVolume24h",
				"min": 500
			},
			"filter8": {
				"type": "maxVolume24h",
				"max": 1000
			},
			"filter9": {
				"type": "minVolatilityPct24h",
				"min": 1
			},
			"filter10": {
				"type": "maxVolatilityPct24h",
				"max": 1
			},
			"filter11": {
				"type": "minSpreadPct",
				"min": 0.00001
			},
			"filter12": {
				"type": "maxSpreadPct",
				"max": 1
			},
			"filter13": {
				"type": "minSlopePctInterval",
				"min": 0.00001
			},
			"filter14": {
				"type": "maxSlopePctInterval",
				"max": 1
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			},
			"filter17": {
				"type": "belowMedianVolume"
			},
			"filter18": {
				"type": "aboveMedianVolume"
			}
		},
		"schedule": "* * * * *",
		"type": "removePairs",
		"snapshots": 10,
		"debug": "true",
		"setVariable": {
			"userVariable1": true
		},
		"enabled": true
	},
	"removePairs2-jobname": {
		"pairs": {
			"exclude": "BNB,XVG",
			"noBag": false,
			"removeDisabled": true,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "exact",
				"ducount": 1
			},
			"filter2": {
				"type": "biggerThan",
				"ducount": 1
			},
			"filter3": {
				"type": "smallerThan",
				"ducount": 1
			},
			"filter4": {
				"type": "compareBigger",
				"ema1": 1,
				"ema2": 1
			},
			"filter5": {
				"type": "compareSmaller",
				"ema1": 1,
				"ema2": 1
			},
			"filter6": {
				"type": "differenceBigger",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter7": {
				"type": "differenceSmaller",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			}
		},
		"schedule": "* * * * *",
		"type": "removePairs2",
		"snapshots": 10,
		"resume": false,
		"debug": "true",
		"setVariable": {
			"userVariable1": true
		},
		"enabled": true
	},
	"changeStrategy-jobname": {
		"pairs": {
			"exclude": "",
			"bag": true,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "minPrice",
				"min": 0.0000001
			},
			"filter2": {
				"type": "maxPrice",
				"max": 0.0000010
			},
			"filter3": {
				"type": "minPricePctChangeInterval",
				"min": 0.00002
			},
			"filter4": {
				"type": "maxPricePctChangeInterval",
				"max": 1
			},
			"filter5": {
				"type": "minVolumePctChangeInterval",
				"min": 10
			},
			"filter6": {
				"type": "maxVolumePctChangeInterval",
				"max": 50
			},
			"filter7": {
				"type": "minVolume24h",
				"min": 500
			},
			"filter8": {
				"type": "maxVolume24h",
				"max": 1000
			},
			"filter9": {
				"type": "minVolatilityPct24h",
				"min": 1
			},
			"filter10": {
				"type": "maxVolatilityPct24h",
				"max": 1
			},
			"filter11": {
				"type": "minSpreadPct",
				"min": 0.00001
			},
			"filter12": {
				"type": "maxSpreadPct",
				"max": 1
			},
			"filter13": {
				"type": "minSlopePctInterval",
				"min": 0.00001
			},
			"filter14": {
				"type": "maxSlopePctInterval",
				"max": 1
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			},
			"filter17": {
				"type": "belowMedianVolume"
			},
			"filter18": {
				"type": "aboveMedianVolume"
			}
		},
		"schedule": "* * * * *",
		"type": "changeStrategy",
		"snapshots": 10,
		"strategy": "baghandler",
		"resume": false,
		"debug": "true",
		"setVariable": {
			"userVariable1": true
		},
		"enabled": true
	},
	"changeStrategy2-jobname": {
		"pairs": {
			"exclude": "",
			"bag": true,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "exact",
				"ducount": 1
			},
			"filter2": {
				"type": "biggerThan",
				"ducount": 1
			},
			"filter3": {
				"type": "smallerThan",
				"ducount": 1
			},
			"filter4": {
				"type": "compareBigger",
				"ema1": 1,
				"ema2": 1
			},
			"filter5": {
				"type": "compareSmaller",
				"ema1": 1,
				"ema2": 1
			},
			"filter6": {
				"type": "differenceBigger",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter7": {
				"type": "differenceSmaller",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			}
		},
		"schedule": "* * * * *",
		"type": "changeStrategy2",
		"snapshots": 10,
		"strategy": "baghandler",
		"resume": false,
		"setVariable": {
			"userVariable1": true
		},
		"debug": "true",
		"enabled": true
	},
	"manageOverrides-jobname": {
		"pairs": {
			"exclude": "DOGE,ETH",
			"include": "USDT,BNB",
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "exact",
				"ducount": 1
			},
			"filter2": {
				"type": "biggerThan",
				"ducount": 1
			},
			"filter3": {
				"type": "smallerThan",
				"ducount": 1
			},
			"filter4": {
				"type": "compareBigger",
				"ema1": 1,
				"ema2": 1
			},
			"filter5": {
				"type": "compareSmaller",
				"ema1": 1,
				"ema2": 1
			},
			"filter6": {
				"type": "differenceBigger",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter7": {
				"type": "differenceSmaller",
				"ema1": 1,
				"ema2": 1,
				"delta": 10
			},
			"filter16": {
				"type": "variableExact",
				"userVar1": false
			}
		},
		"overrides": {
			"DU_BUYDOWN": 3
		},
		"clearOverrides": false,
		"setITB": false,
		"schedule": "*/10 * * * *",
		"type": "manageOverrides",
		"resume": false,
		"setVariable": {
			"userVariable1": true
		},
		"debug": "true",
		"enabled": true
	}
}
```

## Config format definitions

Templates for all available job types and filters.

```javascript
// config templates for different job types
// each job is a "userString", {} element, number of jobs is not limited
// the "type" parameter defines the job type. different job types have (slightly) different config parameters
{
    "addPairs": { // all job names are a user string
        // can use ticker and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "include": "", // must be used
            "maxPairs": 25, // must be used
            "noCrossOver": false, // must be used
            "exchange": "binance", // must be used
            "filteredQuote": [], // can be used, only relevant in BR context. leave out when not used
            "filteredPair": [], // can be used, only relevant in BR context. leave out when not used
            "filteredBase": [] // can be used, only relevant in BR context. leave out when not used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "overrides": {}, // can be used, can be empty when used
        "brStrat": "br", // can be used, only relevant in BR context when using AC for hedging. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "addPairs", // must be used
        "strategy": "gain", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "setITB": true, // must be used
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "removePairs": {
        // can use ticker and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "noBag": false, // must be used
            "removeDisabled": false, // must be used
            "notRemoveBefore": 60, // can be used. leave out when not used
            "removeBase": "USDT", // can be used, only relevant in BR context. leave out when not used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "brStrat": "br", // can be used, only relevant in BR context when using AC for hedging. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "removePairs", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "removePairs2": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "noBag": false, // must be used
            "removeDisabled": false, // must be used
            "notRemoveBefore": 60, // can be used. leave out when not used
            "removeBase": "USDT", // can be used, only relevant in BR context. leave out when not used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "brStrat": "br", // can be used, only relevant in BR context when using AC for hedging. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "removePairs2", // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "changeStrategy": {
        // can use ticker and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "bag": false, // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "changeStrategy", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "changeStrategy2": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "bag": false, // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "changeStrategy2", // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "manageOverrides": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "include": "", // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "overrides": {}, // can be used, can be empty when used
        "clearOverrides": false, // must be used
        "brStrat": "br", // can be used, only relevant in BR context when using AC for hedging. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "manageOverrides", // must be used
        "setITB": true, // must be used
        "delay": 10, // can be used, leave out when not used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "changeDelay": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "include": "", // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "changeDelay", // must be used
        "delay": 10, // can be used, leave out when not used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "manageBotSettings": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "bag": false, // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "bot": {}, // must be used
        "schedule": "* * * * *", // must be used
        "type": "changeStrategy2", // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "hedgeGB": {
        // can use ticker and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "bag": false, // must be used
            "baseFrom": "USDT", // must be used
            "baseTo": "BTC", // must be used
            "hedgePair": "USDT-BTC", // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // must be used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "hedgeGB", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "hedgeGB2": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "bag": false, // must be used
            "baseFrom": "USDT", // must be used
            "baseTo": "BTC", // must be used
            "hedgePair": "USDT-BTC", // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // must be used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "hedgeGB2", // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "hedge": {
        // can use state and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "include": "", // must be used
            "exchange": "binance", // must be used
            "filteredQuote": [], // can be used, only relevant in BR context. leave out when not used
            "filteredPair": [], // can be used, only relevant in BR context. leave out when not used
            "filteredBase": [] // can be used, only relevant in BR context. leave out when not used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "brStrat": "br", // can be used, only relevant in BR context when using AC for hedging. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "hedge", // must be used
        "hedgeTo": "USDT", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    "filteredQuote": {
        // can use ticker and generic filters
        "pairs": {
            "exclude": "", // must be used, can be empty
            "include": "", // must be used
            "exchange": "binance" // must be used
        },
        "filters": {}, // must be used and contain at least 1 filter
        "filters2": {}, // can be used, then must contain at least 1 filter. leave out when not used. filters2 to filters9 can be used in the same way
        "setVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "resetVariable": {
            "userKeys": "userValues"
        }, // can be used, can be empty when used. leave out when not used
        "schedule": "* * * * *", // must be used
        "type": "filteredQuote", // must be used
        "snapshots": 20, // must be used, lowest value: 1
        "history": 10, // can be used, leave out when not used. only used together with "historyInterval"
        "historyInterval": 5, // can be used, leave out when not used. only used together with "history"
        "resume": true, // must be used
        "debug": false, // must be used
        "enabled": true // must be used
    },
    // list of all ticker filters
    "tickerFilters": {
        // ticker filters work by collecting tickers for all pairs on ccxt supported exchanges
        // a filter set can have an "unlimited" number of filters
        // the sequence of elements matters within filters
        "filters": {
            "filter1": { // filter name is a user string
                "type": "minPrice",
                "min": 0.5
            },
            "filter2": {
                "type": "maxPrice",
                "max": 0.5
            },
            "filter3": {
                "type": "minPricePctChangeInterval",
                "min": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter4": {
                "type": "maxPricePctChangeInterval",
                "max": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter5": {
                "type": "minVolumePctChangeInterval",
                "min": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter6": {
                "type": "maxVolumePctChangeInterval",
                "max": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter7": {
                "type": "minVolume24h",
                "min": 0.5
            },
            "filter8": {
                "type": "maxVolume24h",
                "max": 0.5
            },
            "filter9": {
                "type": "minVolatilityPct24h",
                "min": 0.5
            },
            "filter10": {
                "type": "maxVolatilityPct24h",
                "max": 0.5
            },
            "filter11": {
                "type": "minSpreadPct",
                "min": 0.5
            },
            "filter12": {
                "type": "maxSpreadPct",
                "max": 0.5
            },
            "filter13": {
                "type": "minSlopePctInterval",
                "min": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter14": {
                "type": "maxSlopePctInterval",
                "max": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter15": {
                "type": "minStandardDevPctInterval",
                "min": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter16": {
                "type": "maxStandardDevPctInterval",
                "max": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter17": {
                "type": "belowMedianVolume"
            },
            "filter18": {
                "type": "aboveMedianVolume"
            },
            "filter19": {
                "type": "minVolumeRank",
                "min": 0.5
            },
            "filter20": {
                "type": "maxVolumeRank",
                "max": 0.5
            },
            "filter21": {
                "type": "bullishStandardDeviationChannel",
                "range": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter22": {
                "type": "bearishStandardDeviationChannel",
                "range": 0.5,
                "lastSnapshots": 3 // can be used, leave out when not used, value must be lower than job snaphot count
            },
            "filter23": {
                "type": "allowsHedging",
                "BTC": "USDT" // any two user defined base currencies
            },
            "filter24": {
                "type": "volumeTrailing",
                "buyLevel": 50,
                "trailingRange": 8
            },
            "filter25": {
                "type": "buyTrailing",
                "buyLevel": 50,
                "trailingRange": 8
            },
            "filter26": {
                "type": "slopeTrailing",
                "buyLevel": 50,
                "trailingRange": 8
            },
            "filter27": {
                "type": "linearRegressionChannel",
                "range": 0
            }
        }
    },
    // list of all ticker history filters, can be used in any job that allows ticker filters
    "tickerHistoryFilters": {
        // ticker filters work by collecting tickers for all pairs on ccxt supported exchanges
        // a filter set can have an "unlimited" number of filters
        // the sequence of elements matters within filters
        "filters": {
            "filter1": { // filter name is a user string
                "type": "minPriceHistory",
                "min": 0.5, 
                "historySource": 6 // value must be lower than job history value
            },
            "filter2": {
                "type": "maxPriceHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter3": {
                "type": "minPricePctChangeIntervalHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter4": {
                "type": "maxPricePctChangeIntervalHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter5": {
                "type": "minVolumePctChangeIntervalHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter6": {
                "type": "maxVolumePctChangeIntervalHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter7": {
                "type": "minVolume24hHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter8": {
                "type": "maxVolume24hHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter9": {
                "type": "minVolatilityPct24hHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value            
            },
            "filter10": {
                "type": "maxVolatilityPct24hHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter11": {
                "type": "minSpreadPctHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter12": {
                "type": "maxSpreadPctHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter13": {
                "type": "minSlopePctIntervalHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter14": {
                "type": "maxSlopePctIntervalHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter15": {
                "type": "minStandardDevPctIntervalHistory",
                "min": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter16": {
                "type": "maxStandardDevPctIntervalHistory",
                "max": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter17": {
                "type": "bearishStandardDeviationChannelHistory",
                "range": 0.5,
                "historySource": 6 // value must be lower than job history value
            },
            "filter18": {
                "type": "bullishStandardDeviationChannelHistory",
                "range": 0.5,
                "historySource": 6 // value must be lower than job history value
            }
        }
    },
    // list of all pair state filters
    "stateFilters": {
        // state filters read data from the gunbot ledger data, for pairs that are actively cycling
        // a filter set can have an "unlimited" number of filters
        // the sequence of elements matters within filters
        "filters": {
            "filter1": {
                "type": "exact",
                "ducount1": 1 // key is a user string
            },
            "filter2": {
                "type": "biggerThan",
                "ema1": 1 // key is a user string
            },
            "filter3": {
                "type": "smallerThan",
                "ema1": 1 // key is a user string
            },
            "filter4": {
                "type": "compareBigger",
                "ema1": 1, // key is a user string, value is irrelevant but must be set
                "ema2": 1 // key is a user string, value is irrelevant but must be set
            },
            "filter5": {
                "type": "compareSmaller",
                "ema1": 1, // key is a user string, value is irrelevant but must be set
                "ema2": 1 // key is a user string, value is irrelevant but must be set
            },
            "filter6": {
                "type": "differenceBigger",
                "ema1": 1, // key is a user string, value is irrelevant but must be set
                "ema2": 1, // key is a user string, value is irrelevant but must be set
                "delta": 10
            }
        }
    },
    // list of all generic filters
    "genericFilters": {
        // generic filters can be used in all job types. some generic filters depend on a pair actively cycling
        // a filter set can have an "unlimited" number of filters
        // the sequence of elements matters within filters
        "filters": {
            "filter1": {
                "type": "variableExact",
                "key": "value" // key and value are user strings/numbers
            },
            "filter2": {
                "type": "variableNotExist",
                "key": "value" // key is a user string/number, value is irrelevant but must be set
            },
            "filter3": {
                "type": "pairVariableExact",
                "key": "value" // key and value are user strings/numbers
                "exchange": "value" // can be used, leave out when not used. value is a user string
            },
            "filter4": {
                "type": "strategyName",
                "strategy": "value" // value is user string
            },
            "filter5": {
                "type": "minTimeInConfig",
                "min": 1
            },
            "filter6": {
                "type": "maxTimeInConfig",
                "max": 1
            },
            "filter7": {
                "type": "maxSameOrder",
                "lastOrders": 4,
                "trigger": "StrategyBuy" // possible string values: StrategyBuy, StrategySell,  StopLimit ,Close, RTSell, RTBuy, RTBuyback, DCA, Cancelled
            },
            "filter8": {
                "type": "minTimeSinceOrder",
                "min": 60,
                "trigger": "StrategyBuy" // possible string values: StrategyBuy, StrategySell,  StopLimit ,Close, RTSell, RTBuy, RTBuyback, DCA, Cancelled
            },
            "filter9": {
                "type": "maxTimeSinceOrder",
                "max": 60,
                "trigger": "StrategyBuy" // possible string values: StrategyBuy, StrategySell,  StopLimit ,Close, RTSell, RTBuy, RTBuyback, DCA, Cancelled
            }
        }
    }
}
```

## Filterable parameters for state filters

Below is a \(non exhaustive\) list of parameters that can be filtered with state filters. Only the most useful ones are listed. All indicator values depend on the indicator settings you've set in the strategy running the pair.

To find the full list of available items to filter using state filters, open the json state file of the pair you want to perform filtering on. Always make sure to only add filters for existing parameters, some are strategy or exchange dependent.

```text
"ABP": 7814.956108919921,    // average bought price, includes trading fees
"Ask": 9025.99,    // current best ask price
"AskVolume": 5.183191,    // volume on ask side of the order book
"Bid": 9025.17,    // current best bid price
"BidVolume": 5.952429000000001,    // volume on bid side of the order book
"CHIKOU": 9025,    // current chikou value
"FAST_SMA": 0.00007840000000000017,    // Current value for fast SMA
"KIJUN": 9110.439999999999, // current kijun value
"MACD": -92.56199382179693,    // current MACD value
"MACD_HISTOGRAM": -42.404581672042966,    // current MACD historgram value
"MACD_SIGNAL": -50.157412149753966,    // current MACD signal value
"ROE": -337.622777196791,     // Current ROE for position, including leverage (bitmex)
"SLOW_SMA": 0.00007835000000000043,    // Current value for slow SMA
"STOCHRSI": 0,    // current StochRSI value
"STOCH_D": 22.795385433280803,    // current Stoch D value
"STOCH_K": 24.829516212043245,    // current Stoch K value
"TENKAN": 9108,    // current tenkan value
"actualSENKOUSPANA": 9257.8625,    // current senkou span a value
"actualSENKOUSPANB": 9295.955,     // current senkou span b value
"adx": 53.365896826023615,    // current adx value
"atr": 57.69181846913094,     // current atr value
"availableMargin": 0,         // available margin (bitmex)
"avgEntryPrice": 0,           // average entry price for current position (bitmex)
"baseBalance": 3153.05465646, // available base balance
"countsell": 0,               // Number of sell orders since the pair was added.
"currentQty": 0,              // current quantity of a position (bitmex)
"diminus": 41.26673671193246, // current di- value
"diplus": 9.110903255097467,  // current di+ value
"ducount": 0,                 // DU count, equals the number of completed DU rounds
"dudone": false,              // Dudone indicates if max DU count is reached. Resets once a regular buy order has been filled after the DU pair sold. 
"ema1": 9243.02363508863,     // Current EMA1 value
"ema2": 9243.02363508863,     // Current EMA2 value
"forecast": 9024.972507172492,    // Forecast price (using time series analysis)
"highBB": 9405.63185580759,   // Current upper bollinger band value
"liquidationPrice": 0,        // Liquidation price (bitmex)
"lowBB": 9081.778944192416,   // Current lower bollinger band value 
"mfi": 26.93273441417246,     // Current MFI value
"ourBaginBase": 420.673099259907,    // Bag value in base currency
"pKIJUN": 9110.439999999999,  // Previous round kijun value
"pTENKAN": 9108,              // Previous round tenkan value
"pastSENKOUSPANA": 9330.935,  // Previous round senkou span a value
"pastSENKOUSPANB": 9319.82,   // Previous round senkou span b value
"prevAsk": 9026.15,           // Previous round best ask value
"prevBid": 9025.6,            // Previous round best bid value
"quoteBalance": 0.04227457,   // Balance of quote holding
"reversal": false,            // Indicates if reversal trading is active
"rsi":39.883053922751834     // Current RSI value
"rtsoldprice": 10153.55,      // Price or last RT Sell
```

