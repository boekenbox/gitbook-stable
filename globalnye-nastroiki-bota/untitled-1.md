---
description: >-
  Узнайте, как использовать Gunbot для совершения сделок на основе входящих
  уведомлений по электронной почте.
---

# TradingView \(дополнение\)

TradingView - самая активная социальная сеть для трейдеров и инвесторов. TradingView позволяет пользователям создавать и обмениваться техническим анализом и передовыми торговыми стратегиями на своих интерактивных графиках. 

С помощью дополнения Gunbot TradingView вы можете торговать по оповещениям, отправленным из пользовательских стратегий в Tradingview, полностью управляя своей стратегией в TradingView. Gunbot получает торговые сигналы по электронной почте и торгует соответственно. 

Это дополнение также может быть использовано для совершения сделок из произвольных оповещений по электронной почте, TradingView строго не требуется. 

{% hint style="info" %}
Это платное дополнение, доступность которого зависит от типа вашей лицензии.
{% endhint %}

## Setup video - Настройка видео <a id="setup-video"></a>

Прежде чем приступить к настройке оповещений, вам необходимо:

* Данные IMAP для адреса электронной почты, на который вы получаете оповещения от TradingView. 
* Pro подписка на tradingview.com \(работает также с пробной версией\). Бесплатные аккаунты ограничены одним предупреждением.

{% embed url="https://youtu.be/H2EybuYxc3Y" %}

Это видео было сделано для более старой версии Gunbot. Основные шаги все еще применяются.

​Сценарий, использованный в примере:[ Finn's Microprofit Strategy](https://gunthy.org/forum/index.php/topic,1548.0.html)​

{% page-ref page="imap-listener.md" %}

## Содержание оповещений <a id="alert-message-contents"></a>

Предупреждающие сообщения должны быть в следующем формате, чтобы Gunbot действовал на них. Оповещения следуют тому же стандартизированному парному синтаксису, который также применяется для обычного использования Gunbot. 

Торговые лимиты могут быть конкретно определены только в предупреждениях о покупке / длинных, для других предупреждений или предупреждений без указания суммы применяются ограничения, установленные в настройках TradingView. 

_Замените_ `EXCHANGE`_на имя вашей биржи._

**Для всех мест обмена**

| Оповещение | Действие |
| :--- | :--- |
| BUY\_EXCHANGE\_BTC-ETH | Купить ETH с помощью BTC |
| BUY\_EXCHANGE\_BTC-ETH\_0.1 | Покупайте ETH, используя BTC с торговым лимитом 0,1 BTC |
| SELL\_EXCHANGE\_USDT-BTC | Продать BTC за USDT |
| STOPLOSS\_EXCHANGE\_BTC-ETH | Продать ETH за BTC, если срабатывает стоп-лосс |
| BUY\_EXCHANGE\_USDT-BTC\_amount\_rate | Купить BTC, используя USDT на указанную сумму в долларах США по указанному курсу. Требуется отключение заказов на рынке ТВ |
| BUY\_EXCHANGE\_BTC-ETH\_0\_rate | Покупайте BTC, используя USDT для "Покупки по лимитам ТВ-трейдинга", с указанной скоростью. Требуется отключение ордеров на телевизионном рынке. |

**Оповещения о маржинальной торговле**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x41E;&#x43F;&#x43E;&#x432;&#x435;&#x449;&#x435;&#x43D;&#x438;&#x435;</th>
      <th
      style="text-align:left">&#x414;&#x435;&#x439;&#x441;&#x442;&#x432;&#x438;&#x435;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Short order for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Long order for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD_amount</td>
      <td style="text-align:left">Short order for XBT-USD with a specified trading limit</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD_amount</td>
      <td style="text-align:left">Long order for XBT-USD with a specified trading limit</td>
    </tr>
    <tr>
      <td style="text-align:left">LONG_EXCHANGE_XBT-USD_amount_rate</td>
      <td style="text-align:left">
        <p>Long order for XBT-USD with a specified</p>
        <p>trading limit and rate.</p>
        <p>Requires TV market orders to be disabled.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SHORT_EXCHANGE_XBT-USD_0_rate</td>
      <td style="text-align:left">
        <p>Short order for XBT-USD without a specified</p>
        <p>trading limit and with a specified rate.</p>
        <p>Requires TV market orders to be disabled.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">CLOSELONG_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Closes a long position for XBT-USD</td>
    </tr>
    <tr>
      <td style="text-align:left">CLOSESHORT_EXCHANGE_XBT-USD</td>
      <td style="text-align:left">Closes a short position for XBT-USD</td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
Примечание о торговых лимитах 

В Bitmex каждый параметр, связанный с торговыми лимитами для маржинальной торговли, должен быть указан в контрактах.

На всех других поддерживаемых маржинальных биржах каждая настройка, связанная с лимитами маржинальной торговли, должна быть указана в количестве валюты котировки.
{% endhint %}

{% hint style="success" %}
Чтобы проверить оповещения в Bitmex Testnet, вы должны написать такие оповещения, как это: LONG\_BITMEXTESTNET\_XBT-USD
{% endhint %}

## TradingView settings <a id="tradingview-settings"></a>

Для запуска Gunbot с надстройкой TradingView, ниже приведены только соответствующие настройки. Обычная стратегия Gunbot и настройки пары не имеют значения и не используются, если `TV_GB`не включен. 

Откройте настройки, перейдя в **Settings** &gt; **TradingView.**

![&#x41F;&#x430;&#x440;&#x430;&#x43C;&#x435;&#x442;&#x440;&#x44B; &#x43D;&#x430;&#x441;&#x442;&#x440;&#x43E;&#x435;&#x43A; &#x434;&#x43B;&#x44F; &#x434;&#x43E;&#x43F;&#x43E;&#x43B;&#x43D;&#x435;&#x43D;&#x438;&#x44F; TradingView ](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-LmxsdigK3A4nYLDBVSt%2F-LmxsfCzq0HL6LB8-NqG%2Fimage.png?alt=media&token=dd108db5-cd60-45dc-b479-11f14fcdf961)

Торговые лимиты для ордеров на покупку устанавливаются в настройках конфигурации, при желании их можно переопределить, указав торговый лимит в содержании предупреждающего сообщения. 

Распоряжения, размещаемые дополнением TradingView, по умолчанию выставляются как рыночные ордера, чтобы гарантировать их исполнение. При желании вы также можете разрешить ему отправлять лимитные ордера. 

Обязательно добавьте одну пару для любого обмена, на котором вы хотите выполнять оповещения. 

Это может быть любая пара, она не будет использоваться дополнением.

### Gain <a id="gain"></a>

Установите минимальный выигрыш в%, которому должны соответствовать сделки, инициированные TradingView, когда `TV_PROTECTION` включен. 

Когда отправляются предупреждения о продаже TradingView, которые будут иметь более низкое усиление, чем это значение, Gunbot не будет размещать ордер. Используйте это, чтобы предотвратить продажи в убыток.

{% hint style="warning" %}
**Работает только для спот-трейдинга.**
{% endhint %}

**Values:** числовой, представляет процент.

**Default value:** 0.6

Parameter name in `config.js`: `TV_GAIN`

### Market orders <a id="market-orders"></a>

По умолчанию ордера отправляются как рыночные ордера. 

Если вы хотите вместо этого отправлять лимитные ордера, отключите эту опцию. Затем ордера на покупку отправляются по предельной цене лучшего ордера на покупку в книге заказов, ордера на продажу размещаются по курсу наивысшего спроса.

**Values:** true or false

**Default value:** true

Parameter name in `config.js`: `TV_MARKET_ORDERS`

### Trading Limit Buy <a id="trading-limit-buy"></a>

Это значение определяет торговый лимит для каждого ордера на покупку, размещенного через надстройку. 

Значение по умолчанию 0,002 будет размещать заказы 0,002 BTC при использовании на паре BTC-x. 

Когда `TV_PYRAMID`не используется, в уведомлении о продаже будет выставлен ордер на продажу для полного остатка котировки.

**Bitmex**: введите желаемое количество контрактов

**Other margin exchanges:** введите сумму в валюте котировки

**Values:** числовой - представляет сумму в базовой валюте.

**Default value:** 0.002

Parameter name in `config.js`: `TV_TRADING_LIMIT_BUY`

### Trading Limit Buy Pyramid <a id="trading-limit-buy-pyramid"></a>

Это значение определяет торговый лимит для каждого ордера на покупку пирамиды, размещенного через надстройку. 

Значение по умолчанию 0,002 будет размещать заказы 0,002 BTC при использовании на паре BTC-x.

**Bitmex**: введите желаемое количество контрактов

**Other margin exchanges:** введите сумму в валюте котировки

**Values:** числовой - представляет сумму в базовой валюте.

**Default value:** 0.002

Parameter name in `config.js`: `TV_TRADING_LIMIT_BUY_PYRAMID`

### Pyramid <a id="pyramid"></a>

Установка этого значения в true разрешает торговлю пирамидами, сумма для каждого ордера пирамиды определяется `TV_TRADING_LIMIT_SELL`или `TV_TRADING_LIMIT_BUY_PYRAMID`.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TV_PYRAMID`

### Trading Limit Sell <a id="trading-limit-sell"></a>

Это значение определяет торговый лимит для ордеров на продажу, когда TV\_PYRAMID включен. 

Значение по умолчанию 0,002 будет размещать заказы 0,002 BTC при использовании на паре BTC-x.

**Bitmex**: введите желаемое количество контрактов

**Other margin exchanges:** введите сумму в валюте котировки

**Values:** числовой - представляет сумму в базовой валюте.

**Default value:** 0.002

Parameter name in `config.js`: `TV_TRADING_LIMIT_SELL`

### Protection <a id="protection"></a>

При значении true Gunbot проверяет, есть ли общая прибыль перед продажей, как указано в `TV_GAIN`. 

ри значении false Gunbot будет выполнять все оповещения TradingView, не вмешиваясь в пользовательскую стратегию.

{% hint style="warning" %}
**Работает только для спот-трейдинга.**
{% endhint %}

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TV_PROTECTION`

### Close all - Закрыть все <a id="close-all"></a>

Когда эта опция включена, вся позиция будет закрыта при получении команды на закрытие, даже если она больше установленных торговых ограничений.

**Values:** числовой - представляет сумму в базовой валюте.

**Default value:** false

Parameter name in `config.js`: `TV_CLOSE_ALL`

### Trading Limit Cap - Торговый лимит <a id="trading-limit-cap"></a>

Максимальная сумма базовой валюты, которая будет вложена в пару.

{% hint style="warning" %}
**Only works for spot trading.**
{% endhint %}

**Values:** числовой - представляет сумму в базовой валюте.

**Default value:** 0.0001

Parameter name in `config.js`: `TV_TRADING_LIMIT_CAP`

### Stoploss Percentage - Процент стоп-лосса <a id="stoploss-percentage"></a>

Процент ниже средней цены покупки, при которой сигнал на продажу должен переопределять `TV_PROTECTION`и продавать в режиме стоп-лосс.

**Values:** числовой, представляет процент.

**Default value:** 60

Parameter name in `config.js`: `TV_STOPLOSS_PERCENTAGE`

### Trading Limit All-In <a id="trading-limit-all-in"></a>

При значении true каждый ордер на покупку будет использовать весь доступный баланс базовой валюты.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TV_TRADING_LIMIT_ALLIN`

### Retry Order - Повтор ордера <a id="retry-order"></a>

Включите, если у вас есть проблемы с получением нескольких предупреждений. 

Gunbot будет повторять обработку заказов в течение 15 минут.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `RETRY_TV_ORDER`

### TV MVTS <a id="tv-mvts"></a>

Устанавливает порог для ордеров на продажу. Если у вас меньше установленной суммы, ордера на продажу размещаться не будут, и бот снова перейдет в режим покупки.

**Values:** числовой - представляет собой общую стоимость авуаров монет в базовой валюте.

**Default value:** 0.001

Parameter name in `config.js`: `TV_MVTS`

### TV GB <a id="tv-gb"></a>

Включите это, чтобы запускать стратегии Gunbot одновременно с надстройкой TradingView. Таким образом, покупка и продажа с использованием стратегий Gunbot или предупреждений TradingView могут быть смешанными. 

Слушатель IMAP должен быть включен, чтобы использовать эту опцию.

**Values:** true or false

**Default value:** false

Parameter name in `config.js`: `TV_GB`

### TV Leverage <a id="tv-leverage"></a>

Только для маржинальной торговли. Устанавливает кредитное плечо для открытия любой позиции. Установка 0 размещает ордер с кросс-маржей, если ваш обмен поддерживает кросс-плечо. Используйте только те значения, которые поддерживаются вашим обменом.

**Values:** числовой - от 0 до 100

**Default value:** 0

Parameter name in `config.js`: `TV_LEVERAGE`

### TV Lending <a id="tv-lending"></a>

Устанавливает максимально приемлемую ставку кредитования при открытии позиции. 

Значение по умолчанию 0,02 означает 2% в день.

**Values:** числовой

**Default value:** 0.02

Parameter name in `config.js`: `TV_LENDING`

​

