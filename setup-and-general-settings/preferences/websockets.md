# Websockets

Gunbot emits certain data through websockets. Limited documentation for this feature is available on the following repository: [https://github.com/GuntharDeNiro/Gunthy/\#webgui-informations-for-devs-](https://github.com/GuntharDeNiro/Gunthy/#webgui-informations-for-devs-)

To change websocket settings, go to **Settings** &gt; **Websocket**.

![Configuration options for websocket output](../../.gitbook/assets/image%20%2813%29.png)

## Settings descriptions

Below you'll find detailed descriptions of all available parameters for websockets.

### Port \(WS\)

{% tabs %}
{% tab title="Description" %}
Sets the port used for websockets.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a port number

**Default value:** 5001
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `port`
{% endtab %}
{% endtabs %}

### Client Port

{% tabs %}
{% tab title="Description" %}
You can change the client port for third party web interfaces here.
{% endtab %}

{% tab title="Values" %}
**Values:** numerical, represents a port number.

**Default value:** 3000
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `clientport`
{% endtab %}
{% endtabs %}

### Hostname

{% tabs %}
{% tab title="Description" %}
The IP address or hostname to be used for WebSockets. Defaults to your localhost. An external IP can also be set.
{% endtab %}

{% tab title="Values" %}
**Values:** string, represents an IP-address or hostname

**Default value:** 127.0.0.1
{% endtab %}

{% tab title="Name" %}
Parameter name in `config.js`: `hostname`
{% endtab %}
{% endtabs %}

