# Bollinger Bands \(TA\)

​[Полосы Боллинджера](https://en.wikipedia.org/wiki/Bollinger_Bands) указывают на относительно высокие и низкие цены, используя эту информацию, вы можете купить относительно низко и продать относительно высоко.

С помощью этой стратегии вы можете указать, какой процент от нижней Полосы Боллинджера покупать, а какой процент от верхней Полосы Боллинджера выставлять ордер на продажу. 

Заказы размещаются после того, как цены сначала выходят за пределы установленного уровня, а затем возвращаются обратно. 

{% hint style="info" %}
Некоторые дополнительные функции доступны для пользователей Gunbot Standard Edition и выше. Они отмечены ниже. 
{% endhint %}

{% hint style="warning" %}
Защита усиления является необязательной для этой стратегии. 

Имейте в виду, что это может привести к тому, что ордера на продажу будут ниже вашей точки безубыточности.
{% endhint %}

## Торговый пример

![](https://user-images.githubusercontent.com/2372008/47218852-65788400-d3ad-11e8-8100-1ab21cbc70dd.PNG)

_Пример того, как можно торговать по этой стратегии._ [_Details and settings_](https://www.tradingview.com/chart/EOSUSDT/L6342dzc-Bollinger-Bands-TA-Gunbot-trading-strategy/)​

## Как работать с этой стратегией <a id="how-to-work-with-this-strategy"></a>

Приведенная ниже инфографика описывает, что вызывает торговлю с этой стратегией.

![](https://user-images.githubusercontent.com/2372008/40925897-196d6510-681b-11e8-94ac-99757f46ab7b.PNG)

## Параметры стратегии 

Следующие параметры настройки доступны для `BBTA` и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js. 

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары. 

Используя параметры `BUY_METHOD` и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD` и `SELL_METHOD` установлено значение `BBTA`. Допустимые значения - это имена стратегий, перечисленные [здесь](../untitled/trading-methods.md).

## Настройки покупки <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку Эти параметры контролируют исполнение ордеров на покупку при использовании `BBTA` в качестве метода покупки. 

Take Buy доступен только для Gunbot Standard и выше. 

### Buy enabled - Покупка включена 

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### NBA - Не покупать дороже <a id="nba"></a>

«Никогда не покупай выше». Используйте это, чтобы разрешить только заказы на покупку ниже последнего курса продажи. 

Это устанавливает минимальную процентную разницу между последним ордером на продажу и следующей покупкой. Значение по умолчанию 0 отключает эту опцию. 

При значении 1 Gunbot будет выставлять ордера на покупку только тогда, когда критерии стратегии будут соответствовать и цена будет как минимум на 1% ниже последней цены продажи.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `NBA`

### Take Buy - Купить по любому <a id="take-buy"></a>

Если этот параметр включен, Gunbot попытается использовать любой шанс покупки между точкой входа в стратегию и вашим параметром `TBUY_RANGE`

Как только цена Ask падает ниже верхней границы этого диапазона \(называемой «Take Buy»\), она падает с диапазоном `TBUY_RANGE` и размещает ордер на покупку, как только цена Ask пересечет «Take Buy». Подтверждающие показатели используются. 

При использовании `TAKE_BUY`все еще возможны обычные стратегические ордера на покупку. 

Эта опция не должна использоваться вместе с разворотной торговлей.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_BUY`

### TBuy Range - Диапазон покупки <a id="tbuy-range"></a>

Это устанавливает диапазон покупки для `TAKE_BUY`. 

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% выше точки входа, определенной параметром `BUY_LEVEL`

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TBUY_RANGE`

### Buy Level - Точка входа <a id="buy-level"></a>

Это устанавливает цель для покупки в процентах ниже самой низкой EMA. 

Когда вы установите его на 1, точка входа будет установлена на 1% ниже самой низкой на данный момент EMA.

**Values:** числовой, представляет процент.

**Default value:** 1

Parameter name in `config.js`: `BUY_LEVEL`

## Sell settings - Настройки продажи  <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `BBTA` в качестве метода продажи.

### Sell enabled - Продажа включена  <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Take Profit - Получить любую прибыль <a id="take-profit"></a>

Если этот параметр включен, Gunbot будет пытаться получить любую возможную прибыль между точкой безубыточности и точкой выхода вашей стратегии. Это может быть полезно, например, в дни, когда рынки движутся очень медленно. 

Он работает, торгуя цены вверх между точкой безубыточности и точкой выхода стратегии, с настраиваемым диапазоном для трейлинга: `TP_RANGE`. Ордер на продажу будет размещен, когда достигнут предел трейлинг-стопа или достигнуты условия продажи стратегии. Подтверждающие показатели используются. 

Продажа с минимальными потерями возможна при использовании `TAKE_PROFIT`, действующего как своего рода мини-стоп-лосс. 

Эта опция не должна использоваться вместе с разворотной торговлей или `DOUBLE_CHECK_GAIN`

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_PROFIT`

### TP Range - Диапазон продажи с любой прибылью <a id="tp-range"></a>

Это устанавливает диапазон продаж для `TAKE_PROFIT`.

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% ниже точки безубыточности.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TP_RANGE`

### TP Profit Only - Продавать только с прибылью  <a id="tp-profit-only"></a>

Включите это, чтобы разрешить продажу только выше точки безубыточности.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TP_PROFIT_ONLY`

### Double Check Gain - Продажа с убытком <a id="double-check-gain"></a>

Отключите это, чтобы разрешить ордера на продажу ниже точки безубыточности.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`

### Gain - Усиление <a id="gain"></a>

Это устанавливает минимальную цель для продажи, когда `DOUBLE_CHECK_GAIN`включен.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `GAIN`

## Настройки индикаторов <a id="indicator-settings"></a>

Соответствующие индикаторы для торговли с BBTA. 

Эти настройки имеют прямое влияние на торговлю с `BBTA`.

### Период

Это устанавливает период свечи, используемый для торговли, это влияет на все индикаторы в стратегии. 

Используйте только [поддерживаемые значения](https://app.gitbook.com/@inspire/s/inspire/~/drafts/-LzXpzhOq_DI76bCq0j5/kak-rabotat-s-gunbot/untitled/period#podderzhivaemye-znacheniya-perioda). 

Установка короткого периода позволяет вам торговать по более коротким трендам, но помните, что они будут более шумными, чем более длинные периоды.

**Values:** числовой - представляет размер свечи в минутах.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### Low BB - Низкий BB 

Это устанавливает цель для покупки. Отрицательные значения допускаются. 

Бот будет покупать, когда цена достигнет установленного процента от нижней полосы Боллинджера, и цена будет ниже точки входа, как определено `BUY_LEVEL`. 

При значении 0 целью является нижняя полоса Боллинджера. При значении 30 цель находится на 30% выше нижней полосы Боллинджера - верхняя полоса находится на 100% от нижней полосы. Отрицательные значения допускаются.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `LOW_BB`

### High BB - Высокий BB  <a id="high-bb"></a>

Это устанавливает цель для продажи. Отрицательные значения допускаются. 

Бот будет продавать, когда цена достигнет установленного процента от верхней полосы Боллинджера и достигнет `GAIN`. 

Когда установлено значение 0, целью является верхняя полоса Боллинджера \(ну, почти\). При значении 30 цель находится на уровне 30% под верхней полосой Боллинджера - нижняя полоса находится на 100% от верхней полосы. Отрицательные значения допускаются.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `HIGH_BB`

### SMA Period - Период SMA  <a id="sma-period"></a>

Это определяет количество периодов, используемых для расчета полос Боллинджера.

**Values:** числовой - представляет количество свечей.

**Default value:** 50

Parameter name in `config.js`: `SMAPERIOD`

### Standard Deviation - Среднеквадратичное отклонение  <a id="standard-deviation"></a>

Это значение определяет множитель, используемый для расчета полос Боллинджера.

**Values:** числовой \(рекомендуется: от 1,9 до 2,1\) - представляет значение множителя, используемого при расчете полос Боллинджера.

**Default value:** 2

Parameter name in `config.js`: `STDV`

### Slow EMA - Медленная ЕМА  <a id="slow-ema"></a>

Установите это количество свечей, которое вы хотите использовать для медленной EMA. Цена закрытия для каждой свечи используется в медленном расчете EMA. 

Например: если вы установили `PERIOD` на 5 и хотите использовать 2h для медленной EMA - вам нужно установить `EMA1` на 24 \(24 \* 5 минут\).

**Values:** числовой - представляет количество свечей.

**Default value:** 16

Parameter name in `config.js`: `EMA1`

### Fast EMA - Быстрая ЕМА  <a id="fast-ema"></a>

Установите это количество свечей, которое вы хотите использовать для своей быстрой EMA. Цена закрытия для каждой свечи используется в расчете быстрой EMA. 

Например: если вы установили `PERIOD` на 5 и хотите использовать 1h для быстрой EMA - вам нужно установить `EMA2` на 12 \(12 \* 5 минут\).

**Values:** числовой - представляет количество свечей.

**Default value:** 8

Parameter name in `config.js`: `EMA2`

## TrailMe settings - Настройки TrailMe <a id="trailme-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга. 

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

{% page-ref page="../untitled-2.md" %}

## Balance settings <a id="balance-settings"></a>

{% page-ref page="../untitled-6.md" %}

## Confirming indicator + advanced indicator settings <a id="confirming-indicator-advanced-indicator-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-5.md" %}

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-4.md" %}

## Reversal trading settings <a id="reversal-trading-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-3.md" %}

## Misc settings <a id="misc-settings"></a>

{% page-ref page="../untitled-1.md" %}

## Placeholders - Заполнители <a id="placeholders"></a>

Следующие параметры в `config.js` не имеют функции для этой стратегии и действуют как заполнители.

| Параметр | Описание |
| :--- | :--- |
| `ATRX` | Заполнитель. |
| `ATR_PERIOD` | Placeholder. |
| `BUYLVL1` | Placeholder. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
| `BUY_RANGE` | Placeholder. |
| `DISPLACEMENT` | Placeholder. |
| `FAST_SMA` | Placeholder. |
| `ICHIMOKU_PROTECTION` | Placeholder. |
| `KIJUN_CLOSE` | Placeholder. |
| `KIJUN_PERIOD` | Placeholder. |
| `KIJUN_STOP` | Placeholder. |
| `KUMO_CLOSE` | Placeholder. |
| `KUMO_SENTIMENTS` | Placeholder. |
| `KUMO_STOP` | Placeholder. |
| `LEVERAGE` | Placeholder. |
| `LONG_LEVEL` | Placeholder. |
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
| `SENKOUSPAN_PERIOD` | Placeholder. |
| `SHORT_LEVEL` | Placeholder. |
| `SLOW_SMA` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

