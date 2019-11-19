---
description: >-
  Automated config changes for Gunbot. Enables you to automatically add or
  remove trading pairs, or change change pair strategy overrides - using filter
  criteria you define.
---

# AutoConfig

{% hint style="info" %}
Currently there is no GUI for AutoConfig. You'll need to create your own `autoconfig.json` config file, which contains the jobs it should run.

_If you are not comfortable editing config files manually, it's probably a good idea to wait until the GUI supports AutoConfig._
{% endhint %}

Gunbot AutoConfig is a collection of tools you can use to dynamically manage your config files.

You can create "jobs" to do something you would normally do by hand, for example scan an exchange for potential pairs to add, and schedule the job in a cron-like format.

Things you can currently do with AutoConfig:

* **Scan exchanges and automatically add pairs:** for example add pairs with volume &gt; 500 BTC and for which price is rising.
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



**Pair options:**

**include:** included pairs \(processed first\). Any pair on the exchange that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any pair on the exchange that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

**maxPairs:** maximum number of allowed pairs. In case a filter action would result in more pairs than this setting, the config will be filled up to the max number of allowed pairs. Only enabled pairs count towards maxPairs.

#### Other obligatory parameters:

**strategy:** this defines the strategy that will be assigned to pairs added by this job. It must be the exact name of an existing strategy in your `config.js` file.



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
      "type": "addPairs",
      "strategy": "gain"
    }
  }
```

\*\*\*\*

### Removing pairs

**Type names in config:** `removePairs` \(uses [ticker filters](autoconfig.md#ticker-filters)\) or `removePairs2` \(uses [state filters](autoconfig.md#pair-state-filters)\).

You must have at least one pair set per exchange you use this job type on.



**Pair options:**

**exclude**: pairs that should not be scanned for possible removal. Any active pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled in Gunbot\) are basically the list of includes.

**noBag** \(true/false\): when true, only pairs with a balance below `MIN_VOLUME_TO_SELL` that have no open orders and are not in reversal trading, are filtered for possible removal. When set to false, all pairs in config are filtered.

**removeDisabled** \(true/false\): when true, each time a removePairs job is ran it will remove all disabled pairs for the exchange the job runs on - regardless of filter settings. Useful, for example, when you use `COUNT_SELL`.



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
    "type": "removePairs"
  }
}
```

### 

### Change strategy

**Type names in config:** `changeStrategy` \(uses [ticker filters](autoconfig.md#ticker-filters)\) or `changeStrategy2` \(uses [state filters](autoconfig.md#pair-state-filters)\).

_This job type is basically the same as how removePairs works, but this one changes a pairs strategy instead of removing the pair._

You must have at least one pair set per exchange you use this job type on.

\*\*\*\*

**Pair options:**

**exclude**: pairs that should not be scanned for possible removal. Any active pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled in Gunbot\) are basically the list of includes.

**bag** \(true/false\): when true, only pairs with a balance below `MIN_VOLUME_TO_SELL` that have no open orders and are not in reversal trading, are filtered for possible removal. When set to false, all pairs in config are filtered. 

\*\*\*\*

**Other obligatory parameter:**

**strategy:** the target strategy to set for pairs matching all filters.



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
        "type": "changeStrategy",
        "strategy": "baghandler"
    }
}
```

### 

### Managing overrides

**Type name in config:** `manageOverrides`

This job type uses [state filters.](autoconfig.md#pair-state-filters)



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

```text
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

### 

### Change exchange delay

**Type name in config:** `changeDelay`

This job type uses [state filters.](autoconfig.md#pair-state-filters)



**Pair options:**

**include:** included pairs \(processed first\), can not be empty. Any active trading pair that matches any of the includes, will be processed**.** In case you also use the exclude option, the resulting pairs after processing the includes is the starting point for processing excludes, which will remove items from this list of pairs.

Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**exclude**: excluded pairs \(processed last\). Any active trading pair that matches any of the excludes, won't be processed. 

Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

\*\*\*\*

**Other obligatory parameter:**

**delay**: exchange delay in seconds, this value will be set when one or more pairs in the job pass all filters.

\*\*\*\*

#### Config example

The example below shows a job that does the following:

* Scan Binance pairs every minute, process filters on all active trading pairs that include "USDT" or "BNB" and do not include "DOGE" or "ETH" in their pair name.
* Set a the exchange delay for Binance to 10 in case at least one pair matches the filter

```text
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
        "type": "manageOverrides"
  }
}
```



### Optional parameters



**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filter types that include `Interval` in their name. 

For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. 

\*\*\*\*

**overrides**: this job type can also add overrides when it adds new pairs. To do so, add a section with overrides to the job, just like you would in a `manageOverrides` job.

**Bitrage filters:** when used for Bitrage, you can have an addPairs job replace the contents of the exchange filter settings. To do so, add the filters in the pair section of the job as shown below:

```text
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

## Filter options

### Ticker filters

**For job types:** `addPairs`, `removePairs`, `changeStrategy`

You can use the following filter types for adding and removing pairs. Please note that not every filter type works on every exchange, due to the fact that some exchanges don't offer the required data.

![Data availability for certified exchanges.](../.gitbook/assets/image%20%2827%29.png)

_Filters for prices use ask when adding pairs and bid when filtering for removal or changing strategy_

* `minPrice`: filter returns true when price is higher than set.
* `maxPrice`: filter returns true when price is lower than set.
* `minPricePctChangeInterval`: filter returns true if the current price is at least x% higher than the average price of all snapshots. Only executed when max snapshot sample size is reached.
* `maxPricePctChangeInterval`: filter returns true if the current price is at least x% lower than the average price of all snapshots. Only executed when max snapshot sample size is reached.
* `minVolumePctChangeInterval`: filter returns true if the current 24h volume is at least x% higher than the average 24h volume of all snapshots. Only executed when max snapshot sample size is reached.
* `maxVolumePctChangeInterval`: filter returns true if the current 24h volume is at least x% lower than the average 24h volume of all snapshots. Only executed when max snapshot sample size is reached.
* `minVolume24h`: filter returns true if 24h volume is higher than set, volume in base.
* `maxVolume24h`: filter returns true if 24h volume is lower than set, volume in base.
* `minVolatilityPct24h`: filter returns true if 24h price percentage change is higher than set.
* `maxVolatilityPct24h`: filter returns true if 24h price percentage change is lower than set.
* `minSpreadPct`: filter returns true if percentage difference between bid and ask is higher than set.
* `maxSpreadPct`: filter returns true if percentage difference between bid and ask is lower than set.
* `minSlopePctInterval`: filter returns true if the [slope](https://tulipindicators.org/linregslope) for all prices in snapshots is bigger than set.  Slope is expressed as a percentage of the last price. A slope of 1 means that, according to a simple linear regression, the next collected ticker price is likely to be 1% higher than the last one. Only executed when max snapshot sample size is reached.
* `maxSlopePctInterval`: filter returns true if the [slope](https://tulipindicators.org/linregslope) for all prices in snapshots is smaller than set. Slope is expressed as a percentage of the last price. A slope of 1 means that, according to a simple linear regression, the next collected ticker price is likely to be 1% higher than the last one. Only executed when max snapshot sample size is reached.
* `belowMedianVolume`: filter returns true if the base volume for a pair is lower than the median base volume for all pairs with the same base currency on the exchange.
* `aboveMedianVolume`: filter returns true if the base volume for a pair is higher than the median base volume for all pairs with the same base currency on the exchange.
* `minVolumeRank`: filter returns true if a pairs 24h volume rank \(rankings are specific per base\) is higher than set. This filter is only useful if you want to exclude some of the top ranking volume pairs, for example set it to 5 to only allow pairs that have a volume rank of 6 or higher.
* `maxVolumeRank`: filter returns true if a pairs 24h volume rank \(rankings are specific per base\) is lower than set. Setting it to 10, for example, would only include pairs that have a top10 volume ranking.

_Optionally, you can add `"resume": true` to a job that analyses ticker data. This will make sure that no ticker snapshots get lost between Gunbot restarts. Take care with this option in case you've turned off Gunbot for a while, as you would then be using old ticker data to base decision on._

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

History filters take one additional input, defining which history data should be used. the config for a history filter looks like:

```text
"filter": {
				"type": "minPriceHistory",
				"max": 10,
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
				"type": "minVolumeRankHistory",
				"min": 10
			},
			"filter2": {
				"type": "minSlopePctIntervalHistory",
				"min": 1,
			},
			"filter3": {
				"type": "minVolumeRankHistory",
				"min": 10,
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

![Available pair state filters](../.gitbook/assets/image%20%2856%29.png)

Formula used in `differenceBigger`:  
`100 * ((ema2 - ema1) / ema1) > delta`

Formula used in `differenceSmaller`:  
`100 * ((ema2 - ema1) / ema1) < delta`

_Formula examples use ema1 and ema2 like set in the screenshot above. Of course you can compare any two keys. The position of the keys to compare in the config file do matter._

\_\_

### User Variables

Each job can set one or more user defined variables, which can be used to filter on in other jobs.

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

In case you set a variable that was previously set with a different value, the new value will overwrite the old one. Setting one variable has no effect on other possible variables that are already set.

All variables are written to file and are imported anytime Gunbot restarts. Please be aware that this won't always work, file corruption can happen - for example when a write action is happening right in the moment that Gunbot is closed. It's a good idea to not fully depend on saved variables, and run the jobs that set them relatively frequent.

To read a variable, use the filter type `variableExact`. It can be used in all job types.

```text
"filter": {
"type": "variableExact",
"userVariable1": true
}
```

This filter type will return true when `userVariable1` has a value of `true`.

{% hint style="info" %}
Variables are entirely optional. It's no problem when no `setVariable` exists in a job.
{% endhint %}

## Various

* It's fine to schedule many jobs for the same times, but in case multiple of those jobs causes a config change, the first one to finish will write it's changes and the others jobs will need to wait for another chance.
* The output of every job is that the `config.js` file is updated, this will always cause Gunbot to restart.
* Each job can contain an optional "`debug": true` line, when set the job will show detailed console log output for the filters in this job. Only use this in the moment you really need it, it can slow down Gunbot performance significantly due to the large amount of data that needs to be logged to the console.
* Things that make AutoConfig crash: run a job for an exchange for which zero pairs are set in `config.js`, change the exchange for a ticker filtering job while having `"resume": true`, using a non-existent key in a state filter, etc.
* Data is read from either exchange tickers or the internal Gunbot memory with pair state info. To find out which pair state data to filter on is available, look in the pairs state file in the Gunbot `/json` folder.
* Almost every key/value in pair state files can be filtered, as long as they are on the first level \(not inside additional arrays or objects\)

![First level elements like these can be used for filtering.](../.gitbook/assets/image%20%2859%29.png)

![Elements like these cannot be used for filtering.](../.gitbook/assets/image%20%288%29.png)

## Example config with all possible job types and filters

You don't want to use this ever in this form, but use it as reference for how each job can be formatted.

```text
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
            }
            "filter16": {
                "type": "variableExact",
                "userVar1": false
            }
        },
        "schedule": "* * * * *",
        "type": "addPairs",
        "strategy": "gain",
        "snapshots": 2,
        "resume". false,
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
        "resume". false,
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
        "resume". false,
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
        "resume". false,
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
        "resume". false,
        "setVariable": {
            "userVariable1": true
        },
        "debug": "true",
        "enabled": true
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

