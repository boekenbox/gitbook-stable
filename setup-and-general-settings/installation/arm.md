# ARM installation

## Instructions

1. Unpack the release .zip file to a new folder. Then, from a terminal window, make sure Gunbot is executable:

   `chmod +x gunthy-arm`

2. Start running Gunbot with the following command. Keep this terminal window open.

   `./gunthy-arm`

3. Open `localhost:5000` in a browser on the same system to access the Gunbot GUI \(modern browsers recommended, preferably Chrome or Firefox\)

{% hint style="info" %}
Depending on your systems settings, you may need to add a firewall rule to allow for incoming traffic on TCP port 5000.
{% endhint %}

{% hint style="info" %}
### Note for core users

the default setting is that the GUI starts automatically, but pair processing does not. Set `"start": true,` in `config.js` to start processing pairs.
{% endhint %}

{% hint style="danger" %}
### Security notice

Gunbot is intended to run on your local system. Making the Gunbot GUI available from outside networks is inherently risky, only do so on your own responsibility.

Considerable efforts went into securing the GUI, but please understand that achieving 100% security is not realistic.
{% endhint %}

