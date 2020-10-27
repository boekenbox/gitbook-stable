---
description: How trading pair cycle in Gunbot
---

# Pair processing

**Pair processing example for 1 exchange:**

* 2 enabled trading pairs
* Exchange delay set to 5
* "Processing" below means that Gunbot retrieves data and trades when strategy trading conditions happen in that round
* First round of pair 1 is processed
* Waits 5 seconds \(as defined in the exchange delay\)
* First round of pair 2 is processed
* Waits 5 seconds
* Second round of pair 1 is processed
* Waits 5 seconds
* Second round of pair 2 is processed.
* ... continues cycling through further rounds

{% hint style="info" %}
**Parallel processing of multiple exchanges**

If you have trading pairs across multiple exchanges, each exchange will process pairs in parallel. This means that a pair on exchange 1 does not wait for pairs on exchange 2 to be processed first.

Every exchange will individually cycle through enabled pairs like described above. Each exchange can have it's individual delay setting.
{% endhint %}

