# Stepgain

Stepgain следует рыночным ценам и тренду и покупает или продает, как только тренд разворачивается. Предполагая, что разворот тренда \(от нисходящего тренда к восходящему тренду\) обычно происходит вблизи нижней границы ценового движения, это позволяет вам покупать вскоре после того, как цена достигла дна. Аналогичным образом, продажа происходит, когда восходящий тренд превращается в нисходящий.

Покупка с помощью Stepgain основана на [EMA](https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average), что позволяет Gunbot начать поиск разворота тренда после достижения установленного процента ниже минимальной EMA.

Тренды рассчитываются автоматически XTrend и видны в ваших журналах. Результаты XTrend могут отличаться в зависимости от настроек `PERIOD`. Использование XTrend не является обязательным, вы также можете иметь триггер stepgain только на разворотах цены выше / ниже настроенного уровня

При желании вы можете использовать такие индикаторы, как RSI или Stochastic, в качестве подтверждения, чтобы покупать или продавать, только когда происходит разворот тренда и определенный уровень индикатора.

## Торговый пример <a id="trading-example"></a>

![](https://user-images.githubusercontent.com/2372008/47218318-a66f9900-d3ab-11e8-858c-e4d2959a7371.PNG)

Пример того, как можно торговать с помощью стратегии stepgain. [_Details and settings_](https://www.tradingview.com/chart/XLMBTC/SxHYdOCD-Stepgain-Gunbot-trading-strategy/)​

## Как работать с этой стратегией <a id="how-to-work-with-this-strategy"></a>

Приведенная ниже инфографика описывает, что вызывает торговлю с этой стратегией.

![](https://user-images.githubusercontent.com/2372008/40718216-8e63121c-640f-11e8-917d-719104990195.PNG)

_В этом примере и BUYLVL, и SELLLVL будут установлены на 2. Изменение направления движения цены в данном примере предполагает разворот тренда, в действительности не каждое изменение направления цены будет считаться разворотом тренда, поскольку учитывается сила тренда также._

\_\_

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры настройки доступны для `stepgain`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js.

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары.

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD`и `SELL_METHOD`установлено значение `stepgain`. Допустимые значения - это имена стратегий, перечисленные [здесь](../untitled/trading-methods.md).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `stepgain`в качестве метода покупки.

### Buy enabled <a id="buy-enabled"></a>

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### Stepgain Buy Lvl <a id="stepgain-buy-lvl"></a>

Это устанавливает, какой шаг следует учитывать при покупке:

1. Покупайте, когда цена падает ниже `BUYLVL1`и тренд разворачивается или цена достигает `BUYLVL2`. 
2. Покупать, когда цена падает ниже `BUYLVL2`и тренд разворачивается или цена достигает `BUYLVL3`. 
3. Покупать, когда цена падает ниже `BUYLVL3`и тренд разворачивается.

**Values:** 1 / 2 / 3 – представляет шаги.

**Default value:** 1

Parameter name in `config.js`: `BUYLVL`

### Stepgain Buy Lvl 1 <a id="stepgain-buy-lvl-1"></a>

Определяет первый уровень ниже самой низкой EMA, которая будет рассматриваться для покупки. Используется только когда `BUYLVL`установлен на 1.

При значении 1 это означает, что цена должна быть как минимум на 1 процент ниже самой низкой EMA.

**Values:** числовой, представляет процент.

**Default value:** 0.6

Parameter name in `config.js`: `BUYLVL1`

### Stepgain Buy Lvl 2 <a id="stepgain-buy-lvl-2"></a>

Определяет второй уровень ниже самой низкой EMA, которая будет рассматриваться для покупки. Используется, когда `BUYLVL`установлен на 1 или 2.

Если установлено значение 2, это означает, что цена должна быть как минимум на 2 процента ниже самой низкой EMA.

**Values:** числовой, представляет процент.

**Default value:** 2

Parameter name in `config.js`: `BUYLVL2`

### Stepgain Buy Lvl 3 <a id="stepgain-buy-lvl-3"></a>

Определяет третий уровень ниже самой низкой EMA, которая будет рассматриваться для покупки. Используется, когда `BUYLVL`установлен на 2 или 3.

Если установлено значение 10, это означает, что цена должна быть как минимум на 10 процентов ниже самой низкой EMA.

**Values:** числовой, представляет процент.

**Default value:** 70

Parameter name in `config.js`: `BUYLVL3`

### NBA <a id="nba"></a>

"Никогда не покупай выше ". Используйте это, чтобы разрешить ордера на покупку только ниже последнего курса продажи.

Это устанавливает минимальную процентную разницу между последним ордером на продажу и следующей покупкой. Значение по умолчанию 0 отключает эту опцию.

При значении 1 Gunbot будет выставлять ордера на покупку только тогда, когда критерии стратегии будут соответствовать и цена будет как минимум на 1% ниже последней цены продажи.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `NBA`

### Take Buy <a id="take-buy"></a>

Если этот параметр включен, Gunbot попытается использовать любой шанс покупки между точкой входа в стратегию и вашим параметром `TBUY_RANGE`.

Как только цена Ask падает ниже верхней границы этого диапазона \(называемой «Take Buy»\), она падает с диапазоном `TBUY_RANGE`и размещает ордер на покупку, как только цена Ask пересечет «Take Buy».

Подтверждающие показатели используются.

При использовании `TAKE_BUY`все еще возможны обычные стратегические ордера на покупку. Эта опция не должна использоваться вместе с разворотной торговлей.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_BUY`

### TBuy Range <a id="tbuy-range"></a>

Это устанавливает диапазон покупки для `TAKE_BUY`.

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% выше точки входа, определенной параметром `BUY_LEVEL`.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TBUY_RANGE`

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `bb`в качестве метода продажи.

### Sell enabled <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Stepgain Sell Lvl <a id="stepgain-sell-lvl"></a>

Это устанавливает, какой шаг следует рассматривать для продажи:

1. продавать, когда цена поднимается выше `SELLLVL1`и тренд разворачивается или цена достигает `SELLLVL2`. 
2. Продавать, когда цена поднимается выше `SELLLVL2`и тренд разворачивается или цена достигает `SELLLVL3`. 
3. продавать, когда цена поднимается выше `SELLLVL3`и тренд разворачивается.

**Values:** 1 / 2 / 3 – представляет шаги.

**Default value:** 1

Parameter name in `config.js`: `SELLLVL`

### Stepgain Sell Lvl 1 <a id="stepgain-sell-lvl-1"></a>

Определяет первый уровень выше точки безубыточности, которая будет рассматриваться для продажи. Используется только когда `SELLLVL`установлен в 1.

При значении 1 это означает, что цена должна быть как минимум на 1 процент выше средней покупной цены.

**Values:** числовой, представляет процент.

**Default value:** 0.6

Parameter name in `config.js`: `SELLLVL1`

### Stepgain Sell Lvl 2 <a id="stepgain-sell-lvl-2"></a>

Определяет второй уровень выше точки безубыточности, которая будет рассматриваться для продажи. Используется, когда для `SELLLVL`установлено значение 1 или 2.

При значении 2 это означает, что цена должна быть как минимум на 2 процента выше средней покупной цены.

**Values:** числовой, представляет процент.

**Default value:** 2

Parameter name in `config.js`: `SELLLVL2`

### Stepgain Sell Lvl 3 <a id="stepgain-sell-lvl-3"></a>

Определяет третий уровень выше точки безубыточности, которая будет рассматриваться для продажи. Используется, когда для `SELLLVL`установлено значение 2 или 3.

Если установлено значение 10, это означает, что цена должна быть как минимум на 10 процентов выше средней покупной цены.

**Values:** числовой, представляет процент.

**Default value:** 70

Parameter name in `config.js`: `SELLLVL3`

### Take Profit <a id="take-profit"></a>

Если этот параметр включен, Gunbot будет пытаться получить любую возможную прибыль между точкой безубыточности и точкой выхода вашей стратегии. Это может быть полезно, например, в дни, когда рынки движутся очень медленно.

Он работает, торгуя цены вверх между точкой безубыточности и точкой выхода стратегии, с настраиваемым диапазоном для трейлинга: `TP_RANGE`. Ордер на продажу будет размещен, когда достигнут предел трейлинг-стопа или достигнуты условия продажи стратегии.

Подтверждающие показатели используются.

Продажа с минимальными потерями возможна при использовании `TAKE_PROFIT`, действующего как своего рода мини-стоп-лосс. Эта опция не должна использоваться вместе с разворотной торговлей или `DOUBLE_CHECK_GAIN`

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_PROFIT`

### TP Range <a id="tp-range"></a>

Это устанавливает диапазон продаж для `TAKE_PROFIT`.

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% ниже точки безубыточности.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TP_RANGE`

### TP Profit Only <a id="tp-profit-only"></a>

Включите это, чтобы разрешить продажу только выше точки безубыточности.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TP_PROFIT_ONLY`

### Double Check Gain <a id="double-check-gain"></a>

Это дополнительная проверка, которая просматривает вашу недавнюю историю торговли, чтобы убедиться, что `SELLLVL`будет достигнута до размещения ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`

## Indicator settings <a id="indicator-settings"></a>

Соответствующие индикаторы для торговли с прибылью.

Эти настройки оказывают непосредственное влияние на торговлю с `gain`, поскольку `BUY_LEVEL`зависит от EMA.

### Period <a id="period"></a>

Это устанавливает период свечи, используемый для торговли, это влияет на все индикаторы в стратегии.

Используйте только [поддерживаемые значения](../../kak-rabotat-s-gunbot/untitled/period.md).

Установка короткого периода позволяет вам торговать по более коротким трендам, но помните, что они будут более шумными, чем более длинные периоды.

**Values:** числовой - представляет размер свечи в минутах.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### Slow EMA <a id="slow-ema"></a>

Установите это количество свечей, которое вы хотите использовать для медленной EMA. Цена закрытия для каждой свечи используется в медленном расчете EMA.

Например: если вы установили `PERIOD`на 5 и хотите использовать 2h для медленной EMA - вам нужно установить EMA1 на 24 \(24 \* 5 минут\).

**Values:** числовой - представляет количество свечей.

**Default value:** 16

Parameter name in `config.js`: `EMA1`

### Fast EMA <a id="fast-ema"></a>

Установите это количество свечей, которое вы хотите использовать для своей быстрой EMA. Цена закрытия для каждой свечи используется в расчете быстрой EMA.

Например: если вы установили `PERIOD`на 5 и хотите использовать 1h для быстрой EMA - вам нужно установить EMA2 на 12 \(12 \* 5 минут\).

**Values:** числовой - представляет количество свечей.

**Default value:** 8

Parameter name in `config.js`: `EMA2`

### XTrend Enabled <a id="xtrend-enabled"></a>

Отключите это, чтобы использовать stepgain устаревшим способом: действуйте только на разворотах цены - не считая XTrend.

При отключении stepgain будет торговаться, когда произойдет разворот цены выше / ниже установленного уровня.

Когда эта функция включена, stepgain будет торговаться, когда произойдет разворот цены выше / ниже установленного уровня и направление тренда будет подтверждено XTrend.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `XTREND_ENABLED`

## TrailMe settings

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга.

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

Поскольку пошаговая торговля торгуется при смене тренда, не рекомендуется использовать дополнительный трейлинг для обычных ордеров на покупку или продажу.

{% page-ref page="../untitled-2.md" %}

## Balance settings <a id="balance-settings"></a>

{% page-ref page="../untitled-6.md" %}

## Confirming indicator + advanced indicator settings <a id="confirming-indicator-advanced-indicator-settings"></a>

{% page-ref page="../untitled-5.md" %}

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

{% page-ref page="../untitled-4.md" %}

## Reversal trading settings <a id="reversal-trading-settings"></a>

{% page-ref page="../untitled-3.md" %}

## Misc settings <a id="misc-settings"></a>

{% page-ref page="../untitled-1.md" %}

## Placeholders - Заполнители <a id="placeholders"></a>

Следующие параметры в `config.js` не имеют функции для этой стратегии и действуют как заполнители.

| Parameter | Description |
| :--- | :--- |
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUY_LEVEL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `GAIN` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LEVERAGE` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
| `LOW_BB` | Placeholder. |
| `MACD_LONG` | Placeholder. |
| `MACD_SHORT` | Placeholder. |
| `MACD_SIGNAL` | Placeholder. |
| `MAKER_FEES` | Placeholder. |
| `MEAN_REVERSION` | Placeholder. |
| `PP_BUY` | Placeholder. |
| `PP_SELL` | Placeholder. |
| `PRE_ORDER_GAP` | Placeholder. |
| `PRE_ORDER` | Placeholder. |
| `RENKO_ATR` | Placeholder. |
| `RENKO_BRICK_SIZE` | Placeholder. |
| `RENKO_PERIOD` | Placeholder. |
| `ROE_CLOSE` | Placeholder. |
| `ROE_LIMIT` | Placeholder. |
| `ROE_TRAILING` | Placeholder. |
| `ROE` | Placeholder. |
| `RT_BUY_LEVEL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

