---
description: Trading strategies are sets of rules that govern trading execution.
---

# About Gunbot strategies

A strategy in Gunbot is a set of rules for trading, you can assign it to one or more trading pairs and tweak every setting precisely how you want it.

{% hint style="info" %}
**Gunbot strategies follow the AND principle**

Every buy or sell condition you set, must occur in the **same** round of processing.
{% endhint %}

Because strategies are a very long topic, the documentation is split into different pages:

{% page-ref page="trading-methods.md" %}

{% page-ref page="protections.md" %}

{% page-ref page="../regular-strategies-spot-trading/" %}

{% page-ref page="../margin-trading-strategies/" %}

## Presets

You can save as many strategy presets as you want. The built-in presets are generally meant as a good starting point for an individual strategy.

There are built-in presets for every available trading method. Additionally, there are a few pre-tuned presets available:

* **emotionless-medium:** same as the regular emotionless strategy, but less conservative when buying.
* **emotionless-fast:** same as the regular emotionless strategy, but very trigger happy when buying.
* **bbta-tssl:** a combination between buying with BBTA and selling with tssl.
* **gain-stochrsicross:** based on gain, combined with StochRSI in cross mode as  confirming indicator.

