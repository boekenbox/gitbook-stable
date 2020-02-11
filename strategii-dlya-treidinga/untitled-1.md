---
description: Различные дополнительные варианты стратегии.
---

# Misc settings - Разные настройки

## Разные настройки параметров

Эти настройки стратегии позволяют вам контролировать свой стоп-лимит и другие некатегоризованные функции.

### Stop Limit <a id="stop-limit"></a>

Устанавливает стоп-лимит для продажи монеты с расчетным убытком. 

После размещения ордера Stop Limit бот перейдет в режим покупки после прохождения `TRADES_TIMEOUT`и будет покупать снова, когда рыночные условия соответствуют вашей стратегии покупки. 

Установив стоп-лимит на 60, вы гарантируете, что все авуары за монету будут проданы при потере 60% стоимости по сравнению со средней покупной ценой. Например. средняя покупная цена составляет 100, стоп-лимит выполнен на 40, и все активы проданы. 

{% hint style="info" %}
На Bitmex предел остановки установлен как значение ROE. Установка его в 1 приведет к срабатыванию предела остановки, когда ROE достигнет -1. 

Используйте значение, которое включает ваше кредитное плечо. Вместо этого рекомендуется использовать STOP\_BUY / STOP\_SELL, если это возможно. Они размещаются одновременно с открытием позиции.
{% endhint %}

**Values:** числовой - представляет процент.

**Default value:** 60

Parameter name in `config.js`: `STOP_LIMIT`

### SL Disable Buy <a id="sl-disable-buy"></a>

При значении true ордера на покупку будут отключены после того, как пара достигнет `STOP_LIMIT`. 

Для маржинальной торговли ордера на покупку будут отключены, когда длинная позиция достигнет `STOP_LIMIT`.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `SL_DISABLE_BUY`

### SL Disable Sell <a id="sl-disable-sell"></a>

При значении true ордера на продажу будут отключены после того, как короткая позиция достигнет `STOP_LIMIT`. 

Специфично для маржинальной торговли.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `SL_DISABLE_SELL`

### Panic Sell <a id="panic-sell"></a>

Если установлено значение true, все котировки будут продаваться по рыночной стоимости как можно скорее. Это может привести к убыткам! 

Вы должны включить это, только если хотите немедленно продать свои текущие активы. 

Для маржинальной торговли этот параметр удалит все открытые ордера и закроет любую позицию как можно скорее. Пары не отключаются автоматически впоследствии.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `PANIC_SELL`

### Trades Timeout <a id="trades-timeout"></a>

Устанавливает тайм-аут между двумя сделками для одной пары, в это время никакие сделки не будут размещены.

**Values:** числовой - представляет время в секундах.

**Default value:** 0

Parameter name in `config.js`: `TRADES_TIMEOUT`

### Count Sell <a id="count-sell"></a>

Устанавливает максимальное количество ордеров на продажу перед автоматическим отключением пары. 

Установка этого значения в 5 отключает пару после того, как 5 ордеров на продажу стратегии состоялись \(не включая RT\) Никаких дальнейших сделок не произойдет, пока вы снова не включите пару. Счетчик `COUNT_SELL`сбрасывается после повторного включения пары. 

Этот параметр не имеет значения для торговли на Bitmex.

**Values:** числовой - представляет количество стратегических ордеров на продажу.

**Default value:** 9999

Parameter name in `config.js`: `COUNT_SELL`

### Maker Fees <a id="maker-fees"></a>

**Bitmex**: если установлено значение true, лимитные ордера будут выставляться как постовые ордера. Если ордер может быть \(частично\) заполнен немедленно, он будет отменен Bitmex. Используя `PRE_ORDER`, вы можете настроить расстояние до ордера bid / ask, вы должны использовать отрицательное значение для `PRE_ORDER_GAP`только для заказов. 

**Другие биржи:** если установлено значение true, лимитные ордера на покупку размещаются по заявке, а лимитные ордера на продажу - по запросу 

Это увеличивает вероятность того, что сделка совершается с комиссией производителя.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MAKER_FEES`

### Ignore Trades Before - Игнорировать старые сделки <a id="ignore-trades-before"></a>

Необязательный параметр, который заставляет Gunbot не учитывать сделки до установленной отметки времени. 

Используйте его только тогда, когда вы точно знаете, что делаете, и, например, хотите предотвратить запуск RT на паре, в которой последний ордер на продажу привел к убытку. 

Используйте [https://currentmillis.com/](https://currentmillis.com/), чтобы преобразовать удобочитаемое время в метки времени Unix, не забудьте использовать метку времени в миллисекундах.

**Values:** метка времени Unix в миллисекундах

**Default value:** 0

Parameter name in `config.js`: `IGNORE_TRADES_BEFORE`

### Bought price - Покупная цена <a id="bought-price"></a>

Биржи часто больше не предоставляют информацию о заказах по сделкам, которые произошли давным-давно. Этот параметр существует для ручного указания справочной цены за единицу, которую Gunbot должен учитывать при продаже актива, для которого биржа не предоставляет покупной цены. 

Этот параметр должен использоваться только как переопределение. 

Переопределение действительно только тогда, когда с биржи нельзя получить купленную цену. Если вы хотите принудительно переопределить доступную покупную цену, вы можете применить `IGNORE_TRADES_BEFORE` и после этого удалить файл json состояния пар. 

Этот параметр не имеет значения для торговли на Bitmex.

**Values:** числовой, представляет цену за единицу в базовой валюте.

**Default value:** n/a

Parameter name in `config.js`: `BOUGHT_PRICE`

## Order type settings - Настройки типа ордеров <a id="order-type-settings"></a>

На биржах, которые поддерживают рыночные ордера, вы можете выбрать, какие типы ордеров следует отправлять как лимитные или рыночные. 

**Поддерживаемые биржи** 

Binance, Bitfinex, Bitmex, Coinbase Pro, Кракен и Полоникс 

### Market Buy - Рынок Покупки

При значении true стратегические ордера на покупку / продажу будут размещаться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_BUY`

### Market Sell <a id="market-sell"></a>

При значении true стратегические ордера на продажу будут размещаться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_SELL`

### Market RT Buy <a id="market-rt-buy"></a>

При значении true ордера RT\_BUY будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_RTBUY`

### Market RT Sell <a id="market-rt-sell"></a>

При значении true ордера RT\_SELL будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_RTSELL`

### Market Buyback <a id="market-buyback"></a>

При значении true ордера RT Buyback будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_BUYBACK`

### Market DU <a id="market-du"></a>

При значении true ордера DU будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_DU`

### Market Close <a id="market-close"></a>

При значении true ордера на закрытие позиции в Bitmex будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_CLOSE`

### Market Stop <a id="market-stop"></a>

При значении true ордера Stop Position на Bitmex будут выставляться как рыночные ордера.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_STOP`

### Market FOK <a id="market-fok"></a>

При значении true ордера FOK будут выставляться как рыночные ордера. Это относится к заказам, размещенным в`CANCEL_ORDERS_CYCLE_CAP`.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `MARKET_FOK`

