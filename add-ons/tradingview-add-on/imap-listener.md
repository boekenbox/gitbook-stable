# IMAP listener

{% hint style="info" %}
The IMAP listener settings are only needed when you use the TradingView add-on to let Gunbot execute email alerts. 
{% endhint %}

Using these settings, Gunbot connects to the email address where alerts are sent to.

To change them, go to **Settings** &gt; **IMAP Listener**.

![Imap listener settings options](../../.gitbook/assets/image%20%2816%29.png)



## Settings descriptions

Below you'll find detailed descriptions of all available parameters for the IMAP listener. 

### Enabled

{% tabs %}
{% tab title="Description" %}
Set this to true to enable the TradingView plugin. Not all license types support this option.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false 

**Default value:** false
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `enabled`
{% endtab %}
{% endtabs %}

### Authorized froms

{% tabs %}
{% tab title="Description" %}
Sets the email addresses that should be processed for incoming alerts. Make sure that the alert message in contained in the email subject.

  
Multiple address are possible if you enter them like this: \["email-1@mail.com","email-2@mail.com"\]
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents one or more email addresses

**Default value:**  \["noreply@tradingview.com"\]
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `authorized_froms`
{% endtab %}
{% endtabs %}

### User

{% tabs %}
{% tab title="Description" %}
Set the username for the IMAP server, usually your own email address. Gunbot will listen for incoming alerts on this address.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents a username

**Default value:**  YOUREMAIL
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `user`
{% endtab %}
{% endtabs %}

### Password

{% tabs %}
{% tab title="Description" %}
Enter the password for your own email address, used for connecting to the IMAP server.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents a password

**Default value:**  YOURPASSWORD
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `password`
{% endtab %}
{% endtabs %}

### Host

{% tabs %}
{% tab title="Description" %}
The address of the IMAP server that the listener needs to connect to.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents a hostname

**Default value:**  imap.gmail.com
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `host`
{% endtab %}
{% endtabs %}

### Port

{% tabs %}
{% tab title="Description" %}
The port number for the IMAP server.
{% endtab %}

{% tab title="Values" %}
**Values:** number, represents a port number

**Default value:**  993
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `port`
{% endtab %}
{% endtabs %}

### TLS

{% tabs %}
{% tab title="Description" %}
Defines if TLS encryption is used for the IMAP connection.
{% endtab %}

{% tab title="Values" %}
**Values:** true or false 

**Default value:** true
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `tls`
{% endtab %}
{% endtabs %}



