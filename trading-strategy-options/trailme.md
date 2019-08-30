---
description: Price trailing for various types of orders.
---

# TrailMe

TrailMe is a group of features that allows you to trail different types of trades to reach better entry or exit points. This page describes all available configuration options.

Trailing works just like it does for the TSSL strategy, the difference being the starting point of trailing.

Orders resulting from trailing are only placed when the main strategy criteria are met, and confirming indicators \(if any\) allow the order. All these conditions must occur in the same cycle.

## TrailMe settings parameters

### Trail Me Buy

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for strategy buy orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_BUY`
{% endtab %}
{% endtabs %}

### Trail Me Sell

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for strategy sell orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | RT buy |
|  | Strategy buy |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_SELL`
{% endtab %}
{% endtabs %}

### Trail Me DU

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for DU orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| DCA buy | RT buy |
|  | Strategy sell |
|  | Close |
|  | Strategy buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_DU`
{% endtab %}
{% endtabs %}

### Trail Me RT

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for RT\_BUY orders.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT buy | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_RT`
{% endtab %}
{% endtabs %}

### Trail Me RT Sell

{% tabs %}
{% tab title="Description" %}
Use this to enable tssl-style trailing for RT\_SELL orders above the last RT\_BUY rate.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TM_RT_SELL`
{% endtab %}
{% endtabs %}

### Trail Me Buy Range

{% tabs %}
{% tab title="Description" %}
This sets the buy range for TrailMe.

Setting a range of 0.5% at a starting price of 0.1 would set a range between 0.0995 and 0.1005. As long as prices keep moving downwards, the range moves down along with the price.

As soon as prices start going upward, the range freezes and a buy order is placed when the price crosses the upper boundary of the range.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy buy | RT sell |
| RT Buy | Strategy sell |
| DCA buy | Close |
|  | Stop limit |
|  | RT buyback |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_BUY_RANGE`
{% endtab %}
{% endtabs %}

### Trail Me Sell Range

{% tabs %}
{% tab title="Description" %}
This sets the sell range for TrailMe.

Setting a range of 0.5% at a current price of 0.1 would set a range between 0.0995 and 0.1005. As long as prices keep moving upwards, the range moves up along with the price.

As soon as prices start going downward, the range freezes and a sell order is placed when the prices crosses the lower boundary of the range.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| Strategy sell | Strategy buy |
|  | RT buy |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT sell |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_SELL_RANGE`
{% endtab %}
{% endtabs %}

### Trail Me RT Sell Range

{% tabs %}
{% tab title="Description" %}
This sets the sell range for TrailMe.

Setting a range of 0.5% at a current price of 0.1 would set a range between 0.0995 and 0.1005. As long as prices keep moving upwards, the range moves up along with the price.

As soon as prices start going downward, the range freezes and a sell order is placed when the prices crosses the lower boundary of the range.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical – represent a percentage.

**Default value:** 0.5
{% endtab %}

{% tab title="Order types" %}
| Affects | Does not affect |
| :--- | :--- |
| RT sell | Strategy buy |
|  | Strategy sell |
|  | Close |
|  | DCA buy |
|  | Stop limit |
|  | RT buyback |
|  | RT buy |
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `TRAIL_ME_RT_SELL_RANGE`
{% endtab %}
{% endtabs %}

