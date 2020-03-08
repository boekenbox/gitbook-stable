---
description: Options for notifications in the browser interface.
---

# GUI notifications

## GUI settings

The GUI Notifications menu lets you change which notifications are shown.

To change them, go to **Settings** &gt; **GUI Notifications**.

![](../../.gitbook/assets/image%20%2816%29.png)

## Settings descriptions

Below you'll find detailed descriptions of all available parameters for GUI settings. A few advanced settings are only available in the `config.js` file.

### Callback Messages

{% tabs %}
{% tab title="Description" %}
Set this to true to receive callback notifications in the GUI.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `callback`
{% endtab %}
{% endtabs %}

### Error Messages

{% tabs %}
{% tab title="Description" %}
Set this to true to receive error notifications in the GUI.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `error`
{% endtab %}
{% endtabs %}

### Trade Messages

{% tabs %}
{% tab title="Description" %}
Set this to true to receive trade notifications in the GUI.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `trade`
{% endtab %}
{% endtabs %}

### Enabled \(GUI\)

{% tabs %}
{% tab title="Description" %}
Set this to false to disable the GUI.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `enabled`
{% endtab %}
{% endtabs %}

### Start

{% tabs %}
{% tab title="Description" %}
When set to false, Gunbot starts the GUI \(if enabled\) but does not process pairs until the core is started from the GUI.

This setting is toggled with the **START BOT CORE** button in the GUI.

In case you don't want to use the GUI, set this to true.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `start`
{% endtab %}
{% endtabs %}

### Port \(GUI\)

{% tabs %}
{% tab title="Description" %}
The port number for the GUI.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a port number.

**Default value:** 5000
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `port`
{% endtab %}
{% endtabs %}

### Https

{% tabs %}
{% tab title="Description" %}
Set this to true to run the GUI via https only.

This requires that you generate a key pair and save these in your Gunbot folder.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `https`
{% endtab %}
{% endtabs %}

### Key

{% tabs %}
{% tab title="Description" %}
Defines the filename of your private key used for running the GUI via https.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents a filename

**Default value:** localhost.key
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `key`
{% endtab %}
{% endtabs %}

### Cert

{% tabs %}
{% tab title="Description" %}
Defines the filename of your certificate / public key used for running the GUI via https.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents a filename

**Default value:** localhost.cert
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `cert`
{% endtab %}
{% endtabs %}

### Networktraffic

{% tabs %}
{% tab title="Description" %}
Set this to true to show GUI network traffic requests in Gunbot logs.

Can be useful for debug purposes.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `networktraffic`
{% endtab %}
{% endtabs %}

### Login

{% tabs %}
{% tab title="Description" %}
Set this to true to enable password authentication. The password is setup using the GUI.

In case you need to reset your password, set this to false.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `login`
{% endtab %}
{% endtabs %}

### TwoFA

{% tabs %}
{% tab title="Description" %}
Set this to true to enable two factor authentication. This is setup using the GUI.

In case you need to reset 2FA, set this to false.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `twoFA`
{% endtab %}
{% endtabs %}

