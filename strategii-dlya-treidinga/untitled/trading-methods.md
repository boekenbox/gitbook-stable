---
description: В рамках стратегии методы торговли определяют основные триггеры для торговли.
---

# Trading methods - Торговые методы

Стратегия в Gunbot - это набор настроек, которые можно назначить одной или нескольким торговым парам. Эти пары будут торговаться в соответствии с назначенными настройками. Важнейшими факторами стратегии являются методы покупки и продажи, которые определяют основную логику ордеров на покупку и продажу.

У Gunbot есть много разных способов выполнения заказов на покупку и продажу. Их можно свободно комбинировать, когда один метод используется для покупки, а другой - для продажи.

У каждого метода и его вариантов для маржинальной торговли есть своя статья в вики, описывающая точную логику и поясняющая все доступные параметры стратегии.

## Доступные методы покупки и продажи

| Метод | Описнаие \(кратко\) |
| :--- | :--- |
| `ADX` | **Average Directional Index:** использует индикатор ADX для торговли только тогда, когда тренды достаточно сильны. |
| `ATRTS` | **Average True Range Trailing Stop:** использует индикатор ATR для измерения изменений волатильности и торговли на трейлинг-стопе. |
| `bb` | **Bollinger Bands:** покупать и продавать в настраиваемых точках между нижней и верхней полосами Боллинджера. |
| `BBTA` | **Bollinger Bands \(TA\):** покупать и продавать после возвращения в настраиваемых точках между нижней и верхней полосами Боллинджера. |
| `EMASPREAD` | **EMA spread:** использует спред между медленной и быстрой EMA для торговли при изменении направления цены. |
| `emotionless` | **Emotionless:** стратегия "просто работает". Вряд ли какие-либо настраиваемые параметры. Идеально подходит для начинающих трейдеров. |
| `gain` | **Gain:** покупайте с процентом ниже EMA, продавайте, когда достигнут ваш установленный выигрыш. |
| `ichimoku` | **Ichimoku:** Алгоритм торговли на основе облачного индикатора Ишимоку. |
| `MACD` | **Moving average convergence/divergence:** использует индикатор MACD для торговли при изменении импульса. |
| `MACDH` | **Moving average convergence/divergence histogram:** использует гистограмму MACD для торговли при изменении импульса. |
| `pp` | **Pingpong:** установите фиксированную цену покупки и продажи, идеально подходящую для монет, которые находятся в пределах предсказуемого ценового диапазона. |
| `stepgain` | **Stepgain:** как `gain`, но после достижения начального уровня покупки или продажи наблюдатель тренда проверит, будут ли цены дальше снижаться или увеличиваться - удостоверившись, что тренд развернулся, и только тогда, он будет покупать или продавать . |
| `SMACROSS` | **SMA cross:** использует пересечения быстрой и медленной SMA для торговли при изменении направления цены. |
| `tsa` | **Time series an**a**lysis:** пытается предсказать будущие цены и торгует на предсказанных разворотах тренда. |
| `tssl` | **Trailing Stop / Stop Limit:** использует скользящий диапазон рыночных цен для покупки и продажи, следуя оптимальным уровням покупки и продажи. |

**Имейте в виду, что не все комбинации методов покупки / продажи подходят для использования вместе с трейлингом или некоторыми подтверждающими индикаторами.**

Например, использование `MACD`, которое срабатывает только в циклах, где линия MACD пересекает сигнальную линию, вместе со Stochastic в качестве подтверждающего индикатора \(в перекрестном режиме, который также запускается только в циклах с пересечением индикатора\), является плохой комбинацией, поскольку и MACD, и Stochastic должны пересекаться в одном и том же цикле - очень сильно уменьшая ваши возможности для торговли.
