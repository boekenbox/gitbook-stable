---
description: 'Настройки порта Websocket, необходимые только для разработчиков.'
---

# Websockets

Gunbot отправляет определенные данные через веб-сокеты. Ограниченная документация для этой функции доступна в [следующем хранилище](https://github.com/GuntharDeNiro/Gunthy/#webgui-informations-for-devs-)​

Чтобы изменить настройки веб-сокета, перейдите к **Settings** &gt; **Websocket**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-LmxpQwc-gql5PR6Bco2%2F-LmxpRJJWjGejzL0OudE%2Fimage.png?alt=media&token=d21f66a9-4901-42d7-9426-4a34da8b8415)

## **Описание настроек**

Ниже вы найдете подробное описание всех доступных параметров для веб-сокетов.

### Port \(WS\)

{% tabs %}
{% tab title="Описание" %}
Устанавливает порт, используемый для веб-сокетов.
{% endtab %}

{% tab title="Настройка" %}
**Values:** числовой, представляет номер порта

**Default value:** 5001
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `port`
{% endtab %}
{% endtabs %}

### Client Port <a id="client-port"></a>

{% tabs %}
{% tab title="Описание" %}
Вы можете изменить клиентский порт для сторонних веб-интерфейсов здесь.
{% endtab %}

{% tab title="Настройка" %}
**Values:** числовой, представляет номер порта

**Default value:** 3000
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `clientport`
{% endtab %}
{% endtabs %}

### Host Name <a id="hostname"></a>

{% tabs %}
{% tab title="Описание" %}
IP-адрес или имя хоста, который будет использоваться для WebSockets. По умолчанию ваш localhost. Внешний IP также может быть установлен.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляющая IP-адрес или имя хоста

**Default value:** 127.0.0.1
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `hostname`
{% endtab %}
{% endtabs %}

