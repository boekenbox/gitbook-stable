# Pingpong

Эта стратегия может использоваться для пар, которые перемещаются вверх и вниз между предсказуемым ценовым диапазоном в течение более длительных периодов. Вы устанавливаете фиксированные цены, по которым Gunbot должен покупать и продавать, Gunbot будет покупать или продавать, как только точная цель будет достигнута или превышена. 

Чтобы уточнить эту стратегию, доступны другие индикаторы, которые можно использовать как подтверждение как для покупки, так и для продажи. Например, вы можете купить Gunbot по определенной цене, только если RSI 30 или ниже. 

Эта стратегия может также использоваться для торговли исключительно по сигналам от подтверждающих индикаторов, устанавливая нереальные цены покупки и продажи. 

Защита усиления является необязательной для этой стратегии. 

Имейте в виду, что это может привести к тому, что ордера на продажу будут ниже вашей точки безубыточности. Если вы хотите разрешить ордера на продажу с убытком, обязательно установите отрицательное значение для `GAIN`.

## Торговый пример <a id="trading-example"></a>

![](https://user-images.githubusercontent.com/2372008/47226031-c4df8f80-d3bf-11e8-9b8d-485d04365ac2.PNG)

Пример того, как можно торговать по этой стратегии. [_Details and settings_](https://www.tradingview.com/chart/BTCUSD/u0VqADZY-Pingpong-Gunbot-trading-strategy/)​

## Strategy parameters <a id="strategy-parameters"></a>

Следующие параметры настройки доступны для `pp`и могут быть установлены в конфигураторе стратегий графического интерфейса пользователя или в разделе стратегий файла config.js. 

Эти настройки являются глобальными и применяются ко всем парам, использующим эту стратегию. Если вы хотите, чтобы определенный параметр отличался для одной или нескольких пар, используйте переопределение на уровне пары. 

Используя параметры `BUY_METHOD`и `SELL_METHOD`, вы можете комбинировать различные методы покупки и продажи. На этой странице стратегии предполагается, что для `BUY_METHOD`и `SELL_METHOD`задано значение `pp`. Допустимые значения - это имена стратегий, перечисленные [здесь](../untitled/trading-methods.md).

## Buy settings <a id="buy-settings"></a>

Настройки покупки являются основным триггером для заказов на покупку. Эти параметры контролируют исполнение ордеров на покупку при использовании `pp`в качестве метода покупки.

### Buy enabled <a id="buy-enabled"></a>

Установите значение false, чтобы Gunbot не размещал ордера на покупку.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `BUY_ENABLED`

### PP Buy <a id="pp-buy"></a>

Это устанавливает точную целевую цену для размещения ордера на покупку. Заказ на покупку будет размещен, как только эта цена будет достигнута или будет доступна даже лучшая цена. 

Например: при торговле парой BTC-x с `PP_BUY`, установленным в 0,00123456, ордер на покупку будет размещен в первом цикле, где цена спроса равна 0,00123456 или ниже.

**Values:** числовой - представляет цену в базовой валюте.

**Default value:** 0

Parameter name in `config.js`: `PP_BUY`

### NBA <a id="nba"></a>

«Никогда не покупай выше». Используйте это, чтобы разрешить только заказы на покупку ниже последнего курса продажи. 

Это устанавливает минимальную процентную разницу между последним ордером на продажу и следующей покупкой. Значение по умолчанию 0 отключает эту опцию. 

При значении 1 Gunbot будет выставлять ордера на покупку только тогда, когда критерии стратегии будут соответствовать и цена будет как минимум на 1% ниже последней цены продажи.

**Values:** числовой, представляет процент.

**Default value:** 0

Parameter name in `config.js`: `NBA`

## Sell settings <a id="sell-settings"></a>

Настройки продажи являются основным триггером для ордеров на продажу. Эти параметры контролируют исполнение ордеров на продажу при использовании `pp`в качестве метода продажи.

### Sell enabled <a id="sell-enabled"></a>

Установите значение false, чтобы Gunbot не выставлял ордера на продажу.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `SELL_ENABLED`

### PP Sell <a id="pp-sell"></a>

Это устанавливает точную целевую цену для размещения ордера на продажу. Ордер на продажу будет размещен, как только эта цена будет достигнута или будет доступна даже лучшая цена. 

Например: при торговле парой BTC-x с `PP_SELL`, установленным в 0,00123456, ордер на продажу будет размещен в первом цикле, где цена предложения равна 0,00123456 или выше.

**Values:** числовой - представляет цену в базовой валюте.

**Default value:** 0

Parameter name in `config.js`: `PP_SELL`

### Double Check Gain - Двойная проверка усиления <a id="double-check-gain"></a>

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

Поскольку `pp`основывается исключительно на ценах, нет индикаторов, которые напрямую влияют на торговлю с `pp`.

## TrailMe settings - Настройки TrailMe <a id="trailme-settings"></a>

Параметры для настройки дополнительного трейлинга для различных типов ордеров. Трейлинг работает так же, как и для стратегии TSSL, различие является отправной точкой трейлинга. 

Распоряжения, полученные в результате трейлинга, размещаются только при выполнении основных критериев стратегии, и подтверждающие индикаторы \(если таковые имеются\) разрешают ордер. Все эти условия должны происходить в одном и том же цикле.

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
| `BUY_LEVEL` | Placeholder. |
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

