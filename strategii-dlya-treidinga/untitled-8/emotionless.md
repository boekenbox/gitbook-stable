# Emotionless

Стратегия Emotionless полностью настроена и готова к использованию, даже для начинающих трейдеров!

Это должна быть относительно безопасная стратегия со скромными, но устойчивыми преимуществами. С этой стратегией вам не нужно думать о настройке правильных или лучших параметров: все это уже есть. Вам нужно только установить основы, такие как ваш торговый лимит, и выбрать, на каких парах вы хотите торговать. При желании вы можете немного увеличить `GAIN`.

Особенности того, как именно работает эта торговая стратегия, не будут раскрыты.

Некоторые дополнительные функции доступны для пользователей Gunbot Standard Edition и выше. Они отмечены ниже.

![](https://user-images.githubusercontent.com/2372008/44547052-27971080-a71a-11e8-8919-c47ecbfc54ef.png)

График, показывающий реальные сделки по Emotionless. Каждый полученный ордер на продажу ~0.6%

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры доступны для `emotionless` и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js.

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары.

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи.

На этой страничке стратегии предполагается, что для параметров `BUY_METHOD`и `SELL_METHOD`установлено значение `emotionless`. Допустимые значения - это имена стратегий, перечисленные [здесь](https://wiki.gunthy.org/trading-strategy-options/about-gunbot-strategies/trading-methods#available-buy-and-sell-methods).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `emotionless`метода покупки.

### Buy enabled <a id="buy-enabled"></a>

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### Buy Level <a id="buy-level"></a>

Это устанавливает цель для покупки в процентах ниже самой низкой EMA.

Когда вы установите его на 1, цель для заказов на покупку будет на 1% ниже самой низкой на данный момент EMA. Из-за трейлинга, Gunbot может купить немного выше цели.

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

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `emotionless`метода продажи.

### Sell enabled <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### Gain <a id="gain"></a>

Это устанавливает минимальную цель для продажи. Gunbot продаст, как только цена достигнет установленного процента выше точки безубыточности. и `HIGH_BB`достигнут.

Если вы хотите иметь как минимум 2% прибыли на сделку, установите это значение равным 2.

**Values:** числовой, представляет процент.

**Default value:** 0.5

Parameter name in `config.js`: `GAIN`

### Double Check Gain <a id="double-check-gain"></a>

Это дополнительная проверка, которая просматривает вашу недавнюю историю торговли, чтобы убедиться, что `GAIN`будет достигнут до размещения ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `DOUBLE_CHECK_GAIN`

## Indicator settings <a id="indicator-settings"></a>

Релевантные индикаторы для трейдинга без эмоций.

Эти настройки имеют прямое влияние на торговлю с `emotionless`.

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

## TrailMe settings <a id="trailme-settings"></a>

{% hint style="info" %}
This is not available for Gunbot Starter.
{% endhint %}

Эти настройки можно использовать, но они не проверены и не предназначены для использования без эмоций. Используйте на свой риск.

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга.

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

{% page-ref page="../untitled-2.md" %}

## Balance settings <a id="balance-settings"></a>

{% page-ref page="../untitled-6.md" %}

## Confirming indicator + advanced indicator settings <a id="confirming-indicator-advanced-indicator-settings"></a>

{% hint style="info" %}
This is not available for Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-5.md" %}

## Dollar cost avg settings <a id="dollar-cost-avg-settings"></a>

{% hint style="info" %}
This is not available for Gunbot Starter.
{% endhint %}

{% page-ref page="../untitled-4.md" %}

## Reversal trading RT settings <a id="reversal-trading-settings"></a>

Эти настройки можно использовать, но они не проверены и не предназначены для использования без эмоций. Используйте на свой риск.

{% hint style="info" %}
Это не доступно для Gunbot Starter.
{% endhint %}

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
| `TAKE_BUY` | Placeholder. |
| `TAKE_PROFIT` | Placeholder. |
| `TBUY_RANGE` | Placeholder. |
| `TENKAN_CLOSE` | Placeholder. |
| `TENKAN_PERIOD` | Placeholder. |
| `TENKAN_STOP` | Placeholder. |
| `TP_PROFIT_ONLY` | Placeholder. |
| `TP_RANGE` | Placeholder. |
| `TSSL_TARGET_ONLY` | Placeholder. |
| `USE_RENKO` | Placeholder. |

