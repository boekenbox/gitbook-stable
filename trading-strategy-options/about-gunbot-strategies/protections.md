# Protections

Trading methods have a few common protections:

* Some methods will only buy once, other can buy multiple times. This is called "pyramid buying".
* "Buy level" is a setting that protects against buying above the lowest EMA. The equivalents to this for margin trading are called "Long level" and "Short level".
* Many methods won't place a strategy sell order below the break-even point, in some methods this is optional.

See the tables below for details about protections/constraints per method.

##  Protections for regular trading \(spot\)

| Method | Allows for pyramid buying? | Protection against buying above EMA? | Protection against selling below break-even point? |
| :--- | :--- | :--- | :--- |
| `ADX` | no | no | optional |
| `ATRTS` | no | no | yes |
| `bb` | no | yes | yes |
| `BBTA` | no | no | optional |
| `EMASPREAD` | no | no | optional |
| `emotionless` | no | yes | yes |
| `gain` | no | yes | yes |
| `ichimoku` | no | no | optional |
| `MACD` | yes | no | optional |
| `MACDH` | yes | no | optional |
| `pp` | no | no | optional |
| `SMACROSS` | yes | no | optional |
| `stepgain` | no | yes | yes |
| `tsa` | no | no | yes |
| `tssl` | no | yes | yes |

####  

## Protections for margin trading

| Method | Requires `LONG_LEVEL` / `SHORT_LEVEL` confirmation? |
| :--- | :--- |
| `ADX` | no |
| `ATRTS` | no |
| `bb` | yes |
| `BBTA` | no |
| `EMASPREAD` | no |
| `emotionless` | yes |
| `gain` | yes |
| `ichimoku` | no |
| `MACD` | no |
| `MACDH` | no |
| `pp` | no |
| `SMACROSS` | no |
| `stepgain` | yes |
| `tsa` | no |
| `tssl` | yes |

