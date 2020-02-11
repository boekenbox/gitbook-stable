# ATRTS - Средний истинный диапазон

Средний истинный диапазон \([ATR](http://stockcharts.com/school/doku.php?id=chart_school:technical_indicators:average_true_range_atr)\) - это индикатор, который измеряет волатильность. Эта стратегия использует ATR для расчета трейлинг-стопов, чтобы обеспечить сигналы покупки и продажи, когда волатильность увеличивается или уменьшается и пересекает трейлинг-стоп. 

Используя настраиваемый множитель `ATRX` для ATR, рассчитывается нижний и верхний пределы.

* Нижний предел, называемый ATR short, рассчитывается путем вычитания результата `ATRX`из ставки следующего раунда. 
* Верхний предел, называемый ATR long, рассчитывается путем добавления результата `ATRX`к запросу следующего раунда.

Ордер на покупку размещается, когда цена Ask пересекает длинную позицию ATR. 

Ордер на продажу размещается, когда цена предложения пересекает уровень ATR и цена выше точки безубыточности. 

Поскольку ATR не предоставляет информацию о направлении цены, настоятельно рекомендуется использовать дополнительный индикатор импульса, такой как RSI.

## Торговый пример <a id="trading-example"></a>

![](https://user-images.githubusercontent.com/2372008/47224087-f99d1800-d3ba-11e8-8f67-2f611244769b.PNG)

Пример того, как можно торговать по этой стратегии. [_Details and settings_](https://www.tradingview.com/chart/BQXBTC/CGLIN3ce-ATRTS-Gunbot-trading-strategy/)​

## Параметры стратегии 

Следующие параметры настройки доступны для `ATRTS`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js. 

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары. 

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD`и `SELL_METHOD`установлены значения `ATRTS`. Допустимые значения - это имена стратегий, перечисленные [здесь](https://wiki.gunthy.org/trading-strategy-options/about-gunbot-strategies/trading-methods). 

## Настройки покупки 

Настройки покупки являются основным триггером для заказов на покупку Эти параметры контролируют исполнение ордеров на покупку при использовании `ATRTS`в качестве метода покупки. 

### Buy enabled - Покупки включены

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### NBA - Не покупай дороже <a id="nba"></a>

«Никогда не покупай выше». Используйте это, чтобы разрешить только заказы на покупку ниже последнего курса продажи. 

Это устанавливает минимальную процентную разницу между последним ордером на продажу и следующей покупкой. Значение по умолчанию 0 отключает эту опцию. 

При значении 1 Gunbot будет выставлять ордера на покупку только тогда, когда критерии стратегии будут соответствовать и цена будет как минимум на 1% ниже последней цены продажи.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `NBA`

### Take Buy - Купить по любому <a id="take-buy"></a>

Если этот параметр включен, Gunbot попытается использовать любой шанс покупки между точкой входа в стратегию и вашим параметром `TBUY_RANGE`

Как только цена Ask падает ниже верхней границы этого диапазона \(называемой «Take Buy»\), она падает с диапазоном `TBUY_RANGE`и размещает ордер на покупку, как только цена Ask пересечет «Take Buy». Используются подтверждающие показатели. 

При использовании `TAKE_BUY`все еще возможны обычные стратегические ордера на покупку. 

Эта опция не должна использоваться вместе с разворотной торговлей.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_BUY`

### TBuy Range - Покупай в диапазоне цен <a id="tbuy-range"></a>

Это устанавливает диапазон покупки для `TAKE_BUY`.

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% выше точки входа, определенной`BUY_LEVEL`.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TBUY_RANGE`

### Buy Level - Точка входа <a id="buy-level"></a>

Это устанавливает точку входа для `TAKE_BUY`в процентах ниже самой низкой EMA. 

Когда вы установите его на 1, точка входа будет установлена на 1% ниже самой низкой на данный момент EMA.

**Values:** числовой, представляет процент.

**Default value:** 1

Parameter name in `config.js`: `BUY_LEVEL`

## Sell settings - Настройки продажи <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `ATRTS` в качестве метода продажи. 

### Продажи включены 

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Take Profit - Получить любую прибыль <a id="take-profit"></a>

Если этот параметр включен, Gunbot будет пытаться получить любую возможную прибыль между точкой безубыточности и точкой выхода вашей стратегии. Это может быть полезно, например, в дни, когда рынки движутся очень медленно. 

Он работает, торгуя цены вверх между точкой безубыточности и точкой выхода стратегии, с настраиваемым диапазоном для трейлинга: `TP_RANGE`. Ордер на продажу будет размещен, когда достигнут предел трейлинг-стопа или достигнуты условия продажи стратегии. Подтверждающие показатели используются. 

Продажа с минимальными потерями возможна при использовании `TAKE_PROFIT`, действующего как своего рода мини-стоп-лосс. Эта опция не должна использоваться вместе с разворотной торговлей или `DOUBLE_CHECK_GAIN`

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TAKE_PROFIT`

### TP Range - Диапазон продажи с любой прибылью <a id="tp-range"></a>

Это устанавливает диапазон продаж для `TAKE_PROFIT`.

При значении 0,5 начальный трейлинг-стоп устанавливается на 0,5% ниже точки безубыточности.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `TP_RANGE`

### TP Profit Only - Продавать только с прибылью <a id="tp-profit-only"></a>

Включите это, чтобы разрешить продажу только выше точки безубыточности.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TP_PROFIT_ONLY`

### Double Check Gain - Продажа с убытком <a id="double-check-gain"></a>

Отключите это, чтобы разрешить ордера на продажу ниже точки безубыточности.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`

### Gain - Усиление  <a id="gain"></a>

Это устанавливает минимальную цель для продажи, когда `DOUBLE_CHECK_GAIN` включен.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `GAIN`

## Настройки индикаторов <a id="indicator-settings"></a>

Соответствующие индикаторы для торговли с ATRTS

Эти настройки имеют прямое влияние на торговлю с `ATRTS`.

### Период

Это устанавливает период свечи, используемый для торговли, это влияет на все индикаторы в стратегии. 

Используйте только [поддерживаемые значения](https://app.gitbook.com/@inspire/s/inspire/~/drafts/-LzXpzhOq_DI76bCq0j5/kak-rabotat-s-gunbot/untitled/period#podderzhivaemye-znacheniya-perioda). 

Установка короткого периода позволяет вам торговать по более коротким трендам, но помните, что они будут более шумными, чем более длинные периоды.

**Values:** числовой - представляет размер свечи в минутах.

**Default value:** 15

Parameter name in `config.js`: `PERIOD`

### ATRx <a id="atrx"></a>

Это значение определяет множитель, используемый для расчета нижнего и верхнего пределов для торговли с `ATRTS`. 

Если установлено значение 5, ATR умножается на 5, а результат вычитается из ставки следующего раунда и добавляется в следующие раунды с просьбой установить лимиты.

**Values:** числовой - представляет множитель для ATR.

**Default value:** 0.5

Parameter name in `config.js`: `ATRX`

### ATR Period - Период ATR  <a id="atr-period"></a>

Количество периодов, используемых для расчета ATR.

**Values:** числовой, представляет количество периодов.

**Default value:** 14

Parameter name in `config.js`: `ATR_PERIOD`

## TrailMe settings - Настройки TrailMe <a id="trailme-settings"></a>

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга. 

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

## Balance settings - Настройки баланса <a id="balance-settings"></a>

{% page-ref page="../untitled-6.md" %}

## Подтверждающий индикатор + расширенные настройки индикатора

{% page-ref page="../untitled-5.md" %}

## Dollar cost avg settings - Доллар стоит в среднем <a id="dollar-cost-avg-settings"></a>

{% page-ref page="../untitled-4.md" %}

## Reversal trading settings - Настройки разворота <a id="reversal-trading-settings"></a>

{% page-ref page="../untitled-3.md" %}

## Misc settings - Разные настройки <a id="misc-settings"></a>

{% page-ref page="../untitled-1.md" %}

## Placeholders - Заполнители <a id="placeholders"></a>

Следующие параметры в `config.js` не имеют функции для этой стратегии и действуют как заполнители.

| Параметр | Описание |
| :--- | :--- |
| `BUYLVL1` | Заполнитель. |
| `BUYLVL2` | Placeholder. |
| `BUYLVL3` | Placeholder. |
| `BUYLVL` | Placeholder. |
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

