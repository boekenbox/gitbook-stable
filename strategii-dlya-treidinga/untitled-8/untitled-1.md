# Ichimoku

Используя индикатор [Ишимоку](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:ichimoku_cloud), вы можете настроить Gunbot для торговли, когда полная свеча пересекает Kumo, Tenkan-Sen или Kijun-Sen. Вы можете настроить, какая строка должна использоваться для ордеров на покупку и / или продажу. 

Защита усиления является необязательной для этой стратегии. Имейте в виду, что это может привести к тому, что ордера на продажу будут ниже вашей точки безубыточности.

## Как работать с этой стратегией <a id="how-to-work-with-this-strategy"></a>

### Покупка <a id="buying"></a>

Ордер на покупку размещается, когда текущая свеча полностью опускается ниже Кумо, Тенкан-Сен или Киджун-Сен. Вы можете настроить, какая строка будет использоваться для заказов на покупку.

### Продажа <a id="selling"></a>

Ордер на продажу выставляется, когда текущая свеча поднимается выше Кумо, Тенкан-Сен или Киджун-Сен. Вы можете настроить, какая строка должна использоваться для ордеров на продажу. 

Рекомендуется использовать дополнительный индикатор импульса.

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры настройки доступны для `ichimoku`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js. 

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары. 

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD`и `SELL_METHOD`установлено значение `ichimoku`. 

Допустимые значения - это имена стратегий, перечисленные [здесь](../untitled/trading-methods.md).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `ichimoku`в качестве метода покупки.

### Buy enabled <a id="buy-enabled"></a>

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### NBA <a id="nba"></a>

«Никогда не покупай выше». Используйте это, чтобы разрешить только заказы на покупку ниже последнего курса продажи. 

Это устанавливает минимальную процентную разницу между последним ордером на продажу и следующей покупкой. Значение по умолчанию 0 отключает эту опцию. 

При значении 1 Gunbot будет выставлять ордера на покупку только тогда, когда критерии стратегии будут соответствовать и цена будет как минимум на 1% ниже последней цены продажи.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `NBA`

### Take Buy <a id="take-buy"></a>

Если этот параметр включен, Gunbot попытается использовать любой шанс покупки между точкой входа в стратегию и вашим параметром `TBUY_RANGE`. 

Как только цена Ask падает ниже верхней границы этого диапазона \(называемой «Take Buy»\), она падает с диапазоном `TBUY_RANGE`и размещает ордер на покупку, как только цена Ask пересечет «Take Buy». 

Подтверждающие показатели используются. 

При использовании `TAKE_BUY`все еще возможны обычные стратегические ордера на покупку. 

Эта опция не должна использоваться вместе с разворотной торговлей.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_BUY`

### TBuy Range <a id="tbuy-range"></a>

Это устанавливает диапазон покупки для `TAKE_BUY`. 

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% выше точки входа, определенной параметром `BUY_LEVEL`.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TBUY_RANGE`

### Buy Level <a id="buy-level"></a>

Это устанавливает точку входа для `TAKE_BUY`в процентах ниже самой низкой EMA. 

Когда вы установите его на 1, точка входа будет установлена на 1% ниже самой низкой на данный момент EMA.

**Values:** числовой, представляет процент.

**Default value:** 1

Parameter name in `config.js`: `BUY_LEVEL`

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `ichimoku`в качестве метода продажи.

### Sell enabled <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Take Profit <a id="take-profit"></a>

Если этот параметр включен, Gunbot будет пытаться получить любую возможную прибыль между точкой безубыточности и точкой выхода вашей стратегии. Это может быть полезно, например, в дни, когда рынки движутся очень медленно. 

Он работает, торгуя цены вверх между точкой безубыточности и точкой выхода стратегии, с настраиваемым диапазоном для трейлинга: `TP_RANGE`. Ордер на продажу будет размещен, когда достигнут предел трейлинг-стопа или достигнуты условия продажи стратегии. 

Подтверждающие показатели используются. 

Продажа с минимальными потерями возможна при использовании `TAKE_PROFIT`, действующего как своего рода мини-стоп-лосс. 

Эта опция не должна использоваться вместе с разворотной торговлей или `DOUBLE_CHECK_GAIN`

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

Отключите это, чтобы разрешить ордера на продажу ниже точки безубыточности.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`

### Gain <a id="gain"></a>

Это устанавливает минимальную цель для продажи, когда `DOUBLE_CHECK_GAIN`включен.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `GAIN`

## Indicator settings <a id="indicator-settings"></a>

Соответствующие индикаторы для торговли с ишимоку. 

Эти настройки имеют прямое влияние на торговлю с `ichimoku`.

### Period <a id="period"></a>

Это устанавливает период свечи, используемый для торговли, это влияет на все индикаторы в стратегии. 

Используйте только [поддерживаемые значения](../../kak-rabotat-s-gunbot/untitled/period.md). 

Установка короткого периода позволяет вам торговать по более коротким трендам, но помните, что они будут более шумными, чем более длинные периоды.

**Values:** numerical– represents candlestick size in minutes.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### Tenkan Period <a id="tenkan-period"></a>

Установите это количество свечных периодов, которые вы хотите использовать для расчета Tenkan-sen.

**Values:** числовой, представляет количество периодов свечи.

**Default value:** 9

Parameter name in `config.js`: `TENKAN_PERIOD`

### Kijun Period <a id="kijun-period"></a>

Установите это количество свечных периодов, которые вы хотите использовать для расчета Киджун-сен.

**Values:** числовой, представляет количество периодов свечи.

**Default value:** 26

Parameter name in `config.js`: `KIJUN_PERIOD`

### Senkouspan Period <a id="senkouspan-period"></a>

Установите это количество свечных периодов, которые вы хотите использовать для расчета диапазона Сенкоу.

**Values:** числовой, представляет количество периодов свечи.

**Default value:** 52

Parameter name in `config.js`: `SENKOUSPAN_PERIOD`

### Displacement <a id="displacement"></a>

Установите это количество свечных периодов, которые вы хотите использовать для смещения Kumo и Chikou-span.

**Values:** числовой, представляет количество периодов свечи.

**Default value:** 26

Parameter name in `config.js`: `DISPLACEMENT`

### Kumo Buy <a id="kumo-buy"></a>

Включите эту покупку, когда последняя свеча полностью опустится ниже Кумо. 

Не включайте несколько триггеров покупки.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `KUMO_BUY`

### Tenkan Buy <a id="tenkan-buy"></a>

Включите эту покупку, когда последняя свеча полностью опустится ниже Тенкан-сен. 

Не включайте несколько триггеров покупки.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `TENKAN_BUY`

### Kijun Buy <a id="kijun-buy"></a>

Включите эту покупку, когда последняя свеча полностью опустится ниже Киджун-сен. 

Не включайте несколько триггеров покупки.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `KIJUN_BUY`

### Kumo Sell <a id="kumo-sell"></a>

Разрешите продавать, когда последняя свеча движется полностью выше Кумо. 

Не включайте несколько триггеров на продажу.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `KUMO_SELL`

### Tenkan Sell <a id="tenkan-sell"></a>

Разрешите продавать, когда последняя свеча движется полностью выше Тенкан-сен. 

Не включайте несколько триггеров покупки.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `TENKAN_SELL`

### Kijun Sell <a id="kijun-sell"></a>

Разрешите продавать, когда последняя свеча будет полностью выше Киджун-сен. 

Не включайте несколько триггеров на продажу.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `KIJUN_SELL`

### Kumo Sentiments <a id="kumo-sentiments"></a>

Включите этот параметр, чтобы разрешать сделки, только когда они подтверждены облаком. Установите значение false, чтобы игнорировать тренды Kumu. 

При включении ордера на покупку могут выполняться только выше Kumo, а ордера на продажу - только ниже Kumo.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `KUMO_SENTIMENTS`

## TrailMe settings <a id="trailme-settings"></a>

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга. 

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле. 

Стратегические требования для этой стратегии заключаются в том, чтобы последняя свеча была полностью выше / ниже установленной линии ишимоку, что оставляет место для дополнительного трейлинга.

{% page-ref page="../../kak-rabotat-s-gunbot/untitled/trailing.md" %}

## Balance settings <a id="balance-settings"></a>

{% page-ref page="../untitled-6.md" %}

## Confirming indicator + advanced indicator settings <a id="confirming-indicator-advanced-indicator-settings"></a>

{% page-ref page="../untitled-5.md" %}



## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

{% page-ref page="../untitled-4.md" %}

## RT Reversal trading settings <a id="reversal-trading-settings"></a>

{% page-ref page="../untitled-3.md" %}

## Misc settings <a id="misc-settings"></a>

{% page-ref page="../untitled-1.md" %}

## Placeholders - Заполнители <a id="placeholders"></a>

Следующие параметры в `config.js` не имеют функции для этой стратегии и действуют как заполнители.

| Parameter | Description |
| :--- | :--- |
| `ATRX` | Placeholder. |
| `ATR_PERIOD` | Placeholder. |
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `HIGH_BB` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
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
| `SELLLVL1` | Placeholder. |
| `SELLLVL2` | Placeholder. |
| `SELLLVL3` | Placeholder. |
| `SELLLVL` | Placeholder. |
| `SELL_RANGE` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

