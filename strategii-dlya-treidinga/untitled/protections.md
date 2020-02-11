---
description: Различные варианты защиты в разных стратегиях.
---

# Protections - Защита покупки

Методы торговли имеют несколько общих средств защиты:

* Некоторые методы можно купить только один раз, другие можно купить несколько раз. Это называется "pyramid buying" - «покупка пирамиды». 
* "Buy level" - «Уровень покупки» - это параметр, который защищает от покупки выше минимальной EMA. Эквивалентами этого для маржинальной торговли называются «Длинный уровень» и «Короткий уровень» - "Long level" and "Short level".
* Многие методы не размещают стратегический ордер на продажу ниже точки безубыточности, в некоторых это необязательно.

См. Таблицы ниже для получения подробной информации о защите / ограничениях для каждого метода.

## Защита для обычной торговли \(спот\)

| Метод | Позволяет купить пирамиду? | Защита от покупки выше EMA? | Защита от продаж ниже точки безубыточности? |
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

## Защита для маржинальной торговли

| Метод | Требуется подтверждение? `LONG_LEVEL` / `SHORT_LEVEL` |
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

