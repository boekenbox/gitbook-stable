---
description: >-
  Automated config changes for Gunbot. Enables you to automatically add or
  remove trading pairs, or change change pair strategy overrides - using filter
  criteria you define.
---

# AutoConfig

Gunbot AutoConfig is a collection of tools you can use to dynamically manage your config files.

You can create "jobs" to do something you would normally do by hand, for example scan an exchange for potential pairs to add, and schedule the job in a cron-like format.

Things you can currently do with Autoconfig:

* **Scan exchanges and automatically add pairs:** for example add pairs with volume &gt; 500 BTC and for which price is rising.
* **Scan exchanges to remove pairs from your config**: for example remove pairs without quote balance for which volume has dropped below 100 BTC.
* **Change the strategy for pairs from your config:** for example set a bag handling strategy when the pair did buy but prices dropped a lot.
* **Change the exchange delay**.
* **Monitor pair state information and automatically set pair overrides:** for example set a different `DU_BUYDOWN` after the first round of DU happened.

To use AutoConfig, you must have this in your `config.js` file:

```text
"AutoConfig": {
        "enabled": true
    },
```

Additionally, you must create a file named `autoconfig.json` which contains one or more jobs to process. Currently there is no GUI yet to manage this config file, but you can find detailed information on each job type in this article.

## How it works

There is a single config file for AutoConfig \(`autoconfig.json`\), which can contain one or many jobs. As soon as Gunbot starts or the config file is changed, all jobs in the config are scheduled \(any pre-existing scheduled job is removed in this process\).

When a job is processed, changes to `config.js` are only made for pairs that have passed every filter.

* The `autoconfig.json` file can contain one or many jobs, each with its own schedule.
* One job always applies to one exchange.
* One job can have one or many filters.
* Within a job, you can set filters for which pairs it applies.

If a job successfully completes, the changes are written to `config.js` and Gunbot restarts using the new settings. If a job causes no changes, for example because it tries to place already existing overrides, it won't cause a Gunbot restart.

Schedules are set in a format similar to how cron jobs are set. If you're not used to the format, use a website like [https://crontab-generator.org/](https://crontab-generator.org/) to generate it. 

## Job types \(with config examples\)

### Adding pairs

Below is a config example that would scan Binance and add BTC-x pairs when they meet the filter criteria.

You must have at least one pair set per exchange you use this job type on.

Filter options are described later in this article.

**Pair options:**

**exclude**: excluded pairs \(processed last\). Any pair on the exchange that matches any of the excludes, won't be processed. Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

**include:** included pairs \(processed first\). Any pair on the exchange that matches any of the includes, will be processed \(after also processing excluded\). Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**maxPairs:** maximum number of allowed pairs. In case a filter action would result in more pairs than this setting, the config will be filled up to the max number of allowed pairs. Only enabled pairs count towards maxPairs.

#### Other options:

**type:** must be set to `addPairs`

**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filtertypes that include `Interval` in their name. For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. For now, snapshot data gets cleared when Gunbot restarts.

**strategy:** this defines the strategy that will be assigned to pairs added by this job.

```text
{
    "addMoon": {
      "pairs": {
        "exclude": "",
        "include": "BTC-",
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
        }
      },
      "schedule": "* * * * *",
      "type": "addPairs",
      "strategy": "gain",
      "snapshots": 2
    }
  }
```

### Removing pairs

Below is a config example that would scan Binance and remove pairs from your config when they meet the filter criteria. Only the pairs that Gunbot already cycled in it's current session can be filtered.

You must have at least one pair set per exchange you use this job type on.

Filter options are described later in this article.

**Pair options:**

**exclude**: pairs that should not be scanned for possible removal. Any active pair that matches any of the excludes, won't be processed. Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

There is no include options for this filter type. Pairs in your config \(that have already cycled\) are basically the list of includes.

**noBag** \(true/false\): when true, only pairs with a balance below mvts are filtered for possible removal. When set to false, all pairs in config are filtered.

**removeDisabled** \(true/false\): when true, each time a removePairs job is ran it will remove all disabled pairs for the exchange the job runs on - regardless of filter settings. Useful, for example, when you use `COUNT_SELL`

#### Other options:

**type:** must be set to `removePairs` \(can use ticker filters\) or `removePairs2` \(can use state filters\)

**snapshots:** defines how many ticker snapshots are saved to perform calculations on. Relevant for filtertypes that include `Interval` in their name. For example: snapshots is set to 10, this means that the ticker data for the last 10 times the job runs are saved and some of the values in it are used for calculating average values over time. For now, snapshot data gets cleared when Gunbot restarts.

```text
{
  "removeCrap": {
    "pairs": {
      "exclude": "BNB,XVG",
      "noBag": false,
      "removeDisabled": true,
      "exchange": "binance"
    },
    "filters": {
      "filter2": {
        "type": "maxPrice",
        "max": 0.00000002
      }
    },
    "schedule": "* * * * *",
    "type": "removePairs",
    "snapshots": 10
  }
}
```

### Change strategy

This job type is basically the same as how removePairs works, but this one changes a pairs strategy instead of removing the pair.

You must have at least one pair set per exchange you use this job type on.

Filter options are described later in this article.

**Pair options not already described for removePairs:**

**bag** \(true/false\): when true, only pairs with a balance above mvts are filtered for possible removal. When set to false, all pairs in config are filtered.

**strategy:** the target strategy to set for pairs matching all filters.

**type:** must be set to `changeStrategy` \(can use ticker filters\) or `changeStrategy2` \(can use state filters\)

```text
{
	"changeStrat": {
		"pairs": {
			"exclude": "",
			"bag": true,
			"exchange": "binance"
		},
		"filters": {
			"filter1": {
				"type": "minSpreadPct",
				"min": 0.000001
			},
			"filter2": {
				"type": "minVolatilityPct24h",
				"min": -10
			}
		},
		"schedule": "* * * * *",
		"type": "changeStrategy",
		"snapshots": 10,
		"strategy": "baghandler"
	}
}
```

### 

### Managing overrides

Filter options are described later in this article.

**exclude**: excluded pairs \(processed last\). Any active pair that matches any of the excludes, won't be processed. Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

**include:** included pairs \(processed first\). Any active pair that matches any of the includes, will be processed \(after also processing excluded\). Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**overrides**: contain one or more overrides, these will be set for each pair that passes all filters when a job is executed.

**clearOverrides** \(true/false\): when set to true, all existing overrides for a pair are removed before placing the new ones.

**exchange**: must be set to the same value as gunbot calls refers to them in the pairs section.

**type**: must be set to `manageOverrides`

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
                "type": "manageOverrides",
                "ducount": 1
            }
        },
        "overrides": {
            "DU_BUYDOWN": 3
        },
        "clearOverrides": false,
        "schedule": "*/10 * * * *",
        "type": "manageOverrides"
  },
  "DynamicDU2": {
        "pairs": {
            "exclude": "DOGE,ETH",
            "include": "USDT,BNB",
            "exchange": "binance"
        },
        "filters": {
            "ducount": {
                "type": "exact",
                "ducount": 2
            }
        },
        "overrides": {
      "DU_BUYDOWN": 8,
      "GAIN": 1
        },
        "clearOverrides": false,
        "type": "manageOverrides",
        "schedule": "1 * * * *"
  },
  "MMshort": {
        "pairs": {
            "exclude": "",
            "include": "XBT-USD",
            "exchange": "bitmex"
        },
        "filters": {
            "side": {
                "type": "exact",
                "state": "SHORT"
            },
            "liquidationDistance": {
                "type": "differenceSmaller",
                "liquidationPrice": 1,
                "avgEntryPrice": 1,
                "delta": 90
            }
        },
        "overrides": {
            "MAX_SELL": 0
        },
        "clearOverrides": true,
        "type": "manageOverrides",
        "schedule": "* * * * *"
    }
}
```

### Change exchange delay

Filter options are described later in this article. Has exactly the same filter options as `manageOverrides` jobs

**exclude**: excluded pairs \(processed last\). Any active pair that matches any of the excludes, won't be processed. Excluded items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be excluded. Input as comma separated list, does not accept spaces between items. Can be empty.

**include:** included pairs \(processed first\). Any active pair that matches any of the includes, will be processed \(after also processing excluded\). Included items do not need to be whole pair names, as long as part of the string matches an actual pair, it will be included. Input as comma separated list, does not accept spaces between items. Can not be empty.

**delay**: exchange delay in seconds, this value will be set when one or more pairs in the job pass all filters.

**exchange**: must be set to the same value as gunbot calls refers to them in the pairs section.

**type**: must be set to `changeDelay`

## Filter options

### Ticker filters

**For job types:** `addPairs`, `removePairs`, `changeStrategy`

You can use the following filter types for adding and removing pairs. Please note that not every filter type works on every exchange, due to the fact that some exchanges don't offer the required data.

![Data availability for certified exchanges.](../.gitbook/assets/image%20%2827%29.png)

_Filters for prices use ask when adding pairs and bid when filtering for removal._

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

### Pair state filters

For job types: `manageOverrides`, `changeDelay`, `removePairs2`, `changeStrategy2`

![Available pair state filters](../.gitbook/assets/image%20%2856%29.png)

Formula used in `differenceBigger`:  
`100 * ((ema2 - ema1) / ema1) > delta` 

Formula used in `differenceSmaller`:  
`100 * ((ema2 - ema1) / ema1) < delta` 

_Formula examples use ema1 and ema2 like set in the screenshot above. Of course you can compare any two keys. The position of the keys to compare in the config file do matter._

## Various 

* It's fine to schedule many jobs for the same times, but in case multiple of those jobs causes a config change, the first one to finish will write it's changes and the others jobs will need to wait for another chance.
* The output of every job is that the `config.js` file is updated, this will always cause Gunbot to restart.
* Each job can contain an optional "`debug": true` line, when set the job will show detailed console log output for the filters in this job.
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
			}
		},
		"schedule": "* * * * *",
		"type": "addPairs",
		"strategy": "gain",
		"snapshots": 2,
		"debug": "true"
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
			}
		},
		"schedule": "* * * * *",
		"type": "removePairs",
		"snapshots": 10,
		"debug": "true"
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
			}
		},
		"schedule": "* * * * *",
		"type": "removePairs2",
		"snapshots": 10,
		"debug": "true"
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
			}
		},
		"schedule": "* * * * *",
		"type": "changeStrategy",
		"snapshots": 10,
		"strategy": "baghandler",
		"debug": "true"
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
			}
		},
		"schedule": "* * * * *",
		"type": "changeStrategy2",
		"snapshots": 10,
		"strategy": "baghandler",
		"debug": "true"
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
			}
		},
		"overrides": {
			"DU_BUYDOWN": 3
		},
		"clearOverrides": false,
		"schedule": "*/10 * * * *",
		"type": "manageOverrides",
		"debug": "true"
	}
}
```

