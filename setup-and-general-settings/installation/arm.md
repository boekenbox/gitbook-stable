---
description: >-
  Instructions to install Gunbot on an ARM device, for Gunbot Standard and
  higher.
---

# ARM installation

## Instructions

1. Unpack the release .zip file to a new folder. Then, from a terminal window, make sure Gunbot is executable:

   `chmod +x gunthy-arm`

2. Start running Gunbot with the following command. Keep this terminal window open.

   `./gunthy-arm`

3. Open `localhost:5000` in a browser on the same system to access the Gunbot GUI \(modern browsers recommended, preferably Chrome or Firefox\)
4. Make sure to enter your registered ERC-20 wallet \("Gunthy wallet"\) and your [registered API key](../profile-settings/connect-exchange.md) in Gunbot before starting the bot core for the first time.

gunthy-arm is a 32-bit package, if your device runs a 64-bit OS the following steps in a terminal window might help getting it running:

```text
sudo apt-get update 
sudo apt-get install crossbuild-essential-armhf 
sudo apt-get install libatomic1 
sudo apt-get install libatomic1-armhf
./gunthy-arm
```

{% hint style="info" %}
Depending on your systems settings, you may need to add a firewall rule to allow for incoming traffic on TCP port 5000.
{% endhint %}

{% hint style="danger" %}
### Security notice

Gunbot is intended to run on your local system. Making the Gunbot GUI available from outside networks is inherently risky, only do so on your own responsibility.

Considerable efforts went into securing the GUI, but please understand that achieving 100% security is not realistic.
{% endhint %}

