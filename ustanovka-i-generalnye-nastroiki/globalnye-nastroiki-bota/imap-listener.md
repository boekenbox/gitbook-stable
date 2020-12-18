---
description: >-
  Настройки для подключения Gunbot к почтовому ящику IMAP, чтобы получать
  оповещения для дополнения TradingView.
---

# IMAP прослушиватель

{% hint style="info" %}
Настройки прослушивателя IMAP необходимы только в том случае, если вы используете надстройку TradingView, чтобы позволить Gunbot выполнять оповещения по электронной почте.
{% endhint %}

Используя эти настройки, Gunbot подключается к адресу электронной почты, на который отправляются оповещения.

Чтобы изменить их, перейдите к **Settings** &gt; **IMAP Listener**.

![&#x41F;&#x430;&#x440;&#x430;&#x43C;&#x435;&#x442;&#x440;&#x44B; &#x43D;&#x430;&#x441;&#x442;&#x440;&#x43E;&#x435;&#x43A; &#x434;&#x43B;&#x44F; &#x43F;&#x43E;&#x434;&#x43A;&#x43B;&#x44E;&#x447;&#x435;&#x43D;&#x438;&#x44F; Gunbot &#x43A; &#x432;&#x430;&#x448;&#x435;&#x43C;&#x443; &#x43F;&#x43E;&#x447;&#x442;&#x43E;&#x432;&#x43E;&#x43C;&#x443; &#x44F;&#x449;&#x438;&#x43A;&#x443; IMAP.](../../.gitbook/assets/image%20%2816%29%20%281%29.png)

{% hint style="info" %}
Советы для пользователей Gmail

* включить доступ для «[менее безопасных приложений](https://support.google.com/accounts/answer/6010255?hl=en)» 
* используйте [пароль приложения](https://support.google.com/accounts/answer/185833?hl=en), если вы используете 2FA для своей учетной записи Google
{% endhint %}

## Описания настроек

Ниже вы найдете подробное описание всех доступных параметров для прослушивателя IMAP.

### Включен \(прослушиватель IMAP\)

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы включить плагин TradingView. Не все типы лицензий поддерживают эту опцию.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `enabled`
{% endtab %}
{% endtabs %}

### Авторизованные формы

{% tabs %}
{% tab title="Описание" %}
Устанавливает адреса электронной почты, которые должны обрабатываться для входящих оповещений. Убедитесь, что оповещение содержится в теме письма.

Несколько адресов возможны, если вы введете их так: \["email-1@mail.com","email-2@mail.com"\]
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет один или несколько адресов электронной почты

**Default value:** \["noreply@tradingview.com"\]
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `authorized_froms`
{% endtab %}
{% endtabs %}

### User <a id="user"></a>

{% tabs %}
{% tab title="Описание" %}
Установите имя пользователя для сервера IMAP, обычно ваш собственный адрес электронной почты. Gunbot будет прослушивать входящие оповещения на этот адрес.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет имя пользователя

**Default value:** YOUREMAIL
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `user`
{% endtab %}
{% endtabs %}

### Password <a id="password"></a>

{% tabs %}
{% tab title="Описание" %}
Введите пароль для вашего собственного адреса электронной почты, который используется для подключения к серверу IMAP.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет пароль

**Default value:** YOURPASSWORD
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `password`
{% endtab %}
{% endtabs %}

### Host <a id="host"></a>

{% tabs %}
{% tab title="Описание" %}
Адрес сервера IMAP, к которому должен подключиться слушатель.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет имя хоста

**Default value:** imap.gmail.com
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `host`
{% endtab %}
{% endtabs %}

### Port \(IMAP\) <a id="port-imap"></a>

{% tabs %}
{% tab title="Описание" %}
Определяет номер порта для сервера IMAP.
{% endtab %}

{% tab title="Настройка" %}
**Values:** номер, представляет номер порта

**Default value:** 993
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `port`
{% endtab %}
{% endtabs %}

### TLS <a id="tls"></a>

{% tabs %}
{% tab title="Описание" %}
Определяет, используется ли шифрование TLS для соединения IMAP.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `tls`
{% endtab %}
{% endtabs %}

### rejectUnauthorized <a id="rejectunauthorized"></a>

{% tabs %}
{% tab title="Описание" %}
Включите этот параметр, чтобы разрешить подключения к серверам IMAP с использованием сертификата, неизвестного центрам сертификации.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `rejectUnauthorized (child of tlsOptions)`
{% endtab %}
{% endtabs %}

