---
description: Instructions to install Gunbot on a macOS machine.
---

# macOS installation

## Instructions

1. Unpack the release .zip file to a new folder.
2. Start running Gunbot with the following command from a terminal in your Gunbot folder:  `./gunthy-macos`. Keep this terminal window open.
3. Open [localhost:5000](http://localhost:5000) in a browser on the same system to access the Gunbot GUI \(modern browsers recommended, preferably Chrome or Firefox\)
4. Make sure to enter your [registered ERC-20 wallet](../exchange-and-license-settings/gunthy-wallet/) \("Gunthy wallet"\) and your [registered API key](../exchange-and-license-settings/connect-exchange/) in Gunbot before starting the bot core for the first time.

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

## Installation video

{% embed url="https://youtu.be/W2dArdu8jus" caption="" %}

_The video above was made for Gunbot v10, however the basic steps still apply._[    
](https://gunbotbeta.gitbook.io/gunbot-beta/getting-started/installation/download)

