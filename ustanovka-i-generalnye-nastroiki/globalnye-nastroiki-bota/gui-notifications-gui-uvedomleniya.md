---
description: Варианты уведомлений в интерфейсе браузера.
---

# GUI notifications - GUI уведомления

## GUI settings - Настройки графического интерфейса <a id="gui-settings"></a>

Меню уведомлений GUI позволяет вам выбирать, какие уведомления показывать.

Чтобы изменить их, перейдите к **Settings** &gt; **GUI Notifications**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L_Rejuz9K0BDQxSQvUH%2F-LmxpIm5Pvw9RmjpnwCF%2F-LmxpJQMBDI6sjJ-W59z%2Fimage.png?alt=media&token=db3a0763-3bb7-494d-b277-6dfe2cffa379)

## Settings descriptions - Описания настроек <a id="settings-descriptions"></a>

Ниже вы найдете подробное описание всех доступных параметров для настроек графического интерфейса. Несколько расширенных настроек доступны только в файле config.js.

### Callback Messages - Обратные сообщения <a id="callback-messages"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы получать уведомления о обратном вызове в графическом интерфейсе.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) вкл или выкл

**Default value:** false - **Настройка по умолчанию**: выкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `callback`
{% endtab %}
{% endtabs %}

### Error Messages - Сообщения об ошибках <a id="error-messages"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы получать уведомления об ошибках в графическом интерфейсе.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** true - **Настройка по умолчанию**: Вкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `error`
{% endtab %}
{% endtabs %}

### Trade Messages - Торговые сообщения <a id="trade-messages"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы получать торговые уведомления в графическом интерфейсе.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** true - **Настройка по умолчанию**: Вкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `trade`
{% endtab %}
{% endtabs %}

### Enabled \(GUI\) - Включено \(GUI\) <a id="enabled-gui"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение false, чтобы отключить графический интерфейс.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** true - **Настройка по умолчанию**: Вкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `enabled`
{% endtab %}
{% endtabs %}

### Start - Начало <a id="start"></a>

{% tabs %}
{% tab title="Описание" %}
При значении false Gunbot запускает графический интерфейс \(если он включен\), но не обрабатывает пары до тех пор, пока ядро не будет запущено из графического интерфейса.

Этот параметр переключается с помощью кнопки START BOT CORE в графическом интерфейсе.

Если вы не хотите использовать графический интерфейс, установите для него значение true.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** false - **Настройка по умолчанию**: Выкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `start`
{% endtab %}
{% endtabs %}

### Port \(GUI\) <a id="port-gui"></a>

{% tabs %}
{% tab title="Описание" %}
Номер порта для графического интерфейса.
{% endtab %}

{% tab title="Настройка" %}
**Values:** Числовая, представляет номер порта для коннекта GUI

**Default value:** 5000
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `port`
{% endtab %}
{% endtabs %}

### Https <a id="https"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true для запуска графического интерфейса только через https.

Для этого необходимо, чтобы вы сгенерировали пару ключей и сохранили их в папке Gunbot.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** false - **Настройка по умолчанию**: Выкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `https`
{% endtab %}
{% endtabs %}

### Key - Ключ <a id="key"></a>

{% tabs %}
{% tab title="Описание" %}
Определяет имя файла вашего закрытого ключа, используемого для запуска графического интерфейса через https.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет имя файла

**Default value:** localhost.key
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `key`
{% endtab %}
{% endtabs %}

### Cert - сертификат <a id="cert"></a>

{% tabs %}
{% tab title="Описание" %}
Определяет имя файла вашего сертификата / открытого ключа, используемого для запуска GUI через https.
{% endtab %}

{% tab title="Настройка" %}
**Values:** строка, представляет имя файла

**Default value:** localhost.cert
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `cert`
{% endtab %}
{% endtabs %}

### Networktraffic - Сетевой трафик <a id="networktraffic"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы показывать запросы сетевого трафика графического интерфейса в журналах Gunbot.

Может быть полезно для отладки.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** false - **Настройка по умолчанию**: Выкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `networktraffic`
{% endtab %}
{% endtabs %}

### Login <a id="login"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы включить аутентификацию по паролю. Пароль устанавливается с помощью графического интерфейса.

Если вам нужно сбросить пароль, установите значение false.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** true - **Настройка по умолчанию**: Вкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `login`
{% endtab %}
{% endtabs %}

### TwoFA <a id="twofa"></a>

{% tabs %}
{% tab title="Описание" %}
Установите значение true, чтобы включить двухфакторную аутентификацию. Это настройка с использованием графического интерфейса.

Если вам нужно сбросить 2FA, установите для этого параметра значение false.
{% endtab %}

{% tab title="Настройка" %}
**Values:** true or false - **Настройка**: \(правда или ложь\) Вкл или Выкл

**Default value:** false - **Настройка по умолчанию**: Выкл
{% endtab %}

{% tab title="Параметр" %}
Имя параметра в `config.js`: `twoFA`
{% endtab %}
{% endtabs %}

