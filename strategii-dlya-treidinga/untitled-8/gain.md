# Gain

Эта стратегия основана на [EMA](https://en.wikipedia.org/wiki/Moving_average#Exponential_moving_average), позволяя Gunbot покупать, когда цена достигает точки ниже самой низкой EMA, и продавать, как только достигнут определенный процент прибыли.

Чтобы уточнить эту стратегию, доступны другие индикаторы, которые можно использовать как подтверждение как для покупки, так и для продажи.

Например, Gunbot может купить, когда цена ниже EMA, а RSI - 30 или ниже.

{% hint style="info" %}
Некоторые дополнительные функции доступны для пользователей Gunbot Standard Edition и выше. Они отмечены ниже.
{% endhint %}

## Торговый пример <a id="trading-example"></a>

![](https://user-images.githubusercontent.com/2372008/47171108-23e1cd80-d308-11e8-8bf3-22f5e461d8ab.PNG)

Пример того, как можно торговать с помощью стратегии усиления. [_Details and settings_](https://www.tradingview.com/chart/XLMBTC/szZmNtD1-Gain-Gunbot-trading-strategy/)​

## Как работать с этой стратегией <a id="how-to-work-with-this-strategy"></a>

Приведенная ниже инфографика описывает, что вызывает торговлю с этой стратегией.

![](https://user-images.githubusercontent.com/2372008/40631012-76d296c8-62d8-11e8-8f09-6319b349955d.PNG)

Настройки, используемые в примере, не рекомендуются.

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры настройки доступны для `gain`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js.

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары.

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что оба параметра `BUY_METHOD`и `SELL_METHOD`настроены на `gain`. Допустимые значения - это имена стратегий, перечисленные [здесь](https://wiki.gunthy.org/trading-strategy-options/about-gunbot-strategies/trading-methods#available-buy-and-sell-methods).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `gain`в качестве метода покупки.

Take Buy доступен только для Gunbot Standard и выше.

### Buy enabled <a id="buy-enabled"></a>

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### Buy Level <a id="buy-level"></a>

Это устанавливает цель для покупки в процентах ниже самой низкой EMA.

Когда вы установите это значение на 1, ордера на покупку будут размещаться только тогда, когда текущая цена будет как минимум на 1% ниже самой низкой на данный момент EMA.

**Values:** числовой, представляет процент.

**Default value:** 1

Parameter name in `config.js`: `BUY_LEVEL`

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

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `bb`в качестве метода продажи.

### Sell enabled <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Gain <a id="gain"></a>

Это устанавливает цель для продажи. Gunbot продаст, как только цена достигнет установленного процента выше точки безубыточности.

Если вы хотите иметь как минимум 2% прибыли на сделку, установите это значение 2. Ордер на продажу будет размещен, когда текущая цена будет как минимум на 2% выше средней покупной цены.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `GAIN`

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

Это дополнительная проверка, которая просматривает вашу недавнюю историю торговли, чтобы убедиться, что `GAIN`будет достигнут до размещения ордера на продажу.

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

Например: если вы установили `PERIOD`на 5 и хотите использовать 2h для медленной EMA - вам нужно установить `EMA1`на 24 \(24 \* 5 минут\).

**Values:** числовой - представляет количество свечей.

**Default value:** 16

Parameter name in `config.js`: `EMA1`

### Fast EMA <a id="fast-ema"></a>

Установите это количество свечей, которое вы хотите использовать для своей быстрой EMA. Цена закрытия для каждой свечи используется в расчете быстрой EMA.

Например: если вы установили `PERIOD`на 5 и хотите использовать 1h для быстрой EMA - вам нужно установить `EMA2`на 12 \(12 \* 5 минут\).

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

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

## Reversal trading settings <a id="reversal-trading-settings"></a>

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-3.md" %}

## Misc settings <a id="misc-settings"></a>

{% page-ref page="../untitled-1.md" %}

## Чтение журнала <a id="reading-the-log"></a>

Предполагая, что никакие подтверждающие индикаторы или дополнительный трейлинг не используются, это как читать журналы для этой стратегии.

### Условия покупки

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-Lq6kRSU3ZPRtvGkAKJm%2F-Lq6koHv_6VrK-FjjOuo%2Fimage.png?alt=media&token=4fd7a5e9-eca3-4cc2-bae6-2a54b0a4e502)

Эту стратегию следует покупать, когда цена Ask равна или ниже точки входа. Цена точки входа определяется на основе вашего параметра `BUY_LEVEL`.

Он будет покупать только тогда, когда объем котировки будет меньше, чем вы установили для `MIN_VOLUME_TO_SELL`, в противном случае он попытается продать вместо этого.

### Условия продажи

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-Lq6kRSU3ZPRtvGkAKJm%2F-Lq6m0C5odfv6h89UD4k%2Fimage.png?alt=media&token=cdd58112-9caf-4e6d-9e6e-2e9f14c27c77)

Ордер на продажу должен быть размещен, когда цена предложения равна или превышает точку выхода.

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

