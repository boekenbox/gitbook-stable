---
description: 'Within a strategy, trading methods define the primary triggers for trading.'
---

# Trading methods

A strategy in Gunbot is a collection of settings that can be assigned to one or more trading pairs. These pairs will then trade according to the assigned settings. The most important factors of a strategy are the buy and sell methods, these define the main logic for buy and sell orders.

Gunbot has many different methods for executing buy and sell orders. These can be freely combined, where one method is being used for buying and another for selling.

Each method, and their variants for margin trading, has it's own wiki article describing the exact logic and explaining all available strategy parameters.

## Available buy and sell methods

| Method | Description \(short\) |
| :--- | :--- |
| `ADX` | **Average Directional Index:** uses the ADX indicator to trade only when trends are strong enough. |
| `ATRTS` | **Average True Range Trailing Stop:** uses the ATR indicator to measure volatility changes and trade on a trailing stop. |
| `bb` | **Bollinger Bands:** buy and sell at configurable points between the lower and upper Bollinger Bands. |
| `BBTA` | **Bollinger Bands \(TA\):** buy and sell after a reentry at configurable points between the lower and upper Bollinger Bands. |
| `EMASPREAD` | **EMA spread:** uses the spread between slow and fast EMA to trade when price direction changes. |
| `emotionless` | **Emotionless:** "just works" strategy. Hardly any configurable parameters. Perfect for novice traders. |
| `gain` | **Gain:** buy at a percentage below EMA, sell when your set gain is reached. |
| `ichimoku` | **Ichimoku:** trading algorithm based on the Ichimoku cloud indicator. |
| `MACD` | **Moving average convergence/divergence:** uses the MACD indicator to trade when momentum changes. |
| `MACDH` | **Moving average convergence/divergence histogram:** uses the MACD histogram to trade when momentum changes. |
| `pp` | **Pingpong:** set a fixed buy and sell price, perfect for coins that stay within a predictable price range. |
| `stepgain` | **Stepgain:** like `gain` , but after hitting initial buy or sell level a trend watcher will check if prices will further decrease or increase - making sure to only buy or sell when the trend reverses. |
| `SMACROSS` | **SMA cross:** uses crossings of fast and slow SMA to trade when price direction changes. |
| `SupportResistance` | **Support / Resistance:** buy at support, sell at resistance. |
| `tsa` | **Time series an**a**lysis:** tries to predict future prices and trades on predicted trend reversals. |
| `tssl` | **Trailing Stop / Stop Limit:** uses a moving range around market prices for buying and selling, trailing optimal buy and sell levels. |

**Keep in mind that not all combinations of buy/sell methods are a good match to use together with trailing or certain confirming indicators.**

For example using `MACD`, which triggers only in cycles where the MACD line crosses the signal line, together with Stochastic as confirming indicator \(in cross mode, which also only triggers in cycles with an indicator cross\) is a bad combination because both MACD and Stochastic must cross in the same cycle - very much reducing your opportunities for a trade.

