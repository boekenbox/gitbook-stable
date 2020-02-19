# MACDH

Эта стратегия основана на [гистограмме MACD](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:macd-histogram), в которой используются сигналы, которые указывают на пересечение сигнальной линии MACD.

Ордер на покупку размещается, когда гистограмма MACD пересекает нулевую линию. Ордер на продажу будет размещен, когда гистограмма MACD опустится ниже 0.

Чтобы уточнить эту стратегию, доступны другие индикаторы, которые можно использовать как подтверждение как для покупки, так и для продажи. Например, Gunbot может купить на бычьем сигнале, а RSI будет 30 или ниже.

{% hint style="warning" %}
Защита усиления Gain является необязательной для этой стратегии.

Имейте в виду, что это может привести к тому, что ордера на продажу будут ниже вашей точки безубыточности.

Нормальные ограничения, такие как «купи один раз, продай один раз», не применяются к этой стратегии: каждый раз, когда появляются критерии покупки, Gunbot выставляет ордер на покупку так же высоко, как установлено в настройках баланса.
{% endhint %}

## Торговый пример <a id="trading-example"></a>

![](https://user-images.githubusercontent.com/2372008/47293430-6b1ed580-d60a-11e8-9270-821289d9a1ec.PNG)

Пример того, как может торговать с этой стратегией_._ [_Details and settings_](https://www.tradingview.com/chart/XRPUSD/HK0j4emv-MACDH-Gunbot-trading-strategy/)​

## Как работать с этой стратегией <a id="how-to-work-with-this-strategy"></a>

Приведенная ниже инфографика описывает, что вызывает торговлю с этой стратегией.

![](https://user-images.githubusercontent.com/2372008/41107941-a4e7da44-6a73-11e8-9cfd-861e4dc12a46.PNG)

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры настройки доступны для `MACDH`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js.

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары.

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD`и `SELL_METHOD`установлено значение `MACDH`. Допустимые значения - это имена стратегий, перечисленные [здесь](../untitled/trading-methods.md).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `MACDH`в качестве метода покупки.

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

### Single Buy <a id="single-buy"></a>

Включите это, чтобы ограничить `MACDH`размещением только одного ордера на покупку в первом перекрестке, после чего он перейдет в режим продажи.

Полезно, когда вы комбинируете `MACDH`с другим методом продажи или используете другие параметры, которые предотвращают продажу ордеров при каждом пересечении.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `SINGLE_BUY`

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

### Buy Level <a id="buy-level"></a>

Это устанавливает точку входа для `TAKE_BUY` в процентах ниже самой низкой EMA. Когда вы установите его на 1, точка входа будет установлена на 1% ниже самой низкой на данный момент EMA.

**Values:** числовой, представляет процент.

**Default value:** 1

Parameter name in `config.js`: `BUY_LEVEL`

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `MACDH`в качестве метода продажи.

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

Соответствующие индикаторы для торговли с MACDH.

Эти настройки напрямую влияют на торговлю с `MACDH`.

### Period <a id="period"></a>

Это устанавливает период свечи, используемый для торговли, это влияет на все индикаторы в стратегии.

Используйте только [поддерживаемые значения](../../kak-rabotat-s-gunbot/untitled/period.md).

Установка короткого периода позволяет вам торговать по более коротким трендам, но помните, что они будут более шумными, чем более длинные периоды.

**Values:** числовой - представляет размер свечи в минутах.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### MACD Short <a id="macd-short"></a>

Количество периодов, используемых для расчета краткосрочной скользящей средней для MACD.

**Values:** числовой, представляет количество периодов.

**Default value:** 5

Parameter name in `config.js`: `MACD_SHORT`

### MACD Long <a id="macd-long"></a>

Количество периодов, используемых для расчета долгосрочной скользящей средней для MACD.

**Values:** числовой, представляет количество периодов.

**Default value:** 20

Parameter name in `config.js`: `MACD_LONG`

### MACD Signal <a id="macd-signal"></a>

Количество периодов, используемых для расчета сигнальной линии MACD.

**Values:** числовой, представляет количество периодов.

**Default value:** 10

Parameter name in `config.js`: `MACD_SIGNAL`

## TrailMe settings - Настройки TrailMe <a id="trailme-settings"></a>

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга.

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

Поскольку эта стратегия торгует на кроссах, происходящих в одном цикле, не рекомендуется использовать дополнительный трейлинг для обычных ордеров на покупку и продажу. Сделки будут происходить только тогда, когда трейлинг-стоп и пересечение стратегии происходят в одном раунде.

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
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `FAST_SMA` | Placeholder. |
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
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

