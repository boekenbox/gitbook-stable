---
description: 'To use Gunbot, an ERC-20 compatible wallet is required.'
---

# GUNTHY wallet

Gunbot uses a blockchain based license validation system. It uses our own ERC-20 utility token called "GUNTHY". Using this system, you can manage your own API keys and change them anytime, or even sell your license to a third party.

**Your wallet address, combined with an airdropped amount of tokens, is your Gunbot license key.**

{% hint style="warning" %}
**You are responsible for your own API keys and GUNTHY wallet.**

Please understand that we might not be able to help you if you lose all your registered API keys and \(access to\) your wallet address. Make sure to follow all the security advise when setting up a wallet and properly save and backup the API key\(s\) used for Gunbot.
{% endhint %}

## Token airdrops and requirements

After you've registered your wallet address, you'll receive an airdrop with the required amount of tokens for your license type. **Hold these tokens at all times to be sure your Gunbot can run.**

These are the required amounts per license:

* **Starter**: 400 GUNTHY
* **Standard**: 1000 GUNTHY
* **Pro**: 1500 GUNTHY
* **Ultimate**: 2500 GUNTHY

Tokens are generally airdropped within 24 hours after registering the wallet address.

In periods when there are discounts available for Gunbot, it's possible that different token requirements apply for users with a discounted license.

## Registering API keys to a GUNTHY wallet address

{% hint style="success" %}
New users register the wallet address and API keys at time of purchase, the steps below only apply for existing users.

For new customers, only the wallet address and API keys need to be entered in Gunbot.
{% endhint %}

To switch to the new license system, please follow these easy steps.

**Step 1. Create a GUNTHY wallet.**

GUNTHY is an ERC-20 token, this means that most Ethereum wallets are compatible. You need to use a wallet you own, a wallet address at an exchange will not work.

Use this smart contract to add the GUNTHY token to your wallet: [https://etherscan.io/address/0x3684b581db1f94b721ee0022624329feb16ab653](https://etherscan.io/address/0x3684b581db1f94b721ee0022624329feb16ab653)

Contract address: `0x3684b581db1f94b721ee0022624329feb16ab653`

If your wallet software asks about the number of decimals to use for GUNTHY, enter 18.

Read more about the [steps to create a GUNTHY wallet](how-to-create-a-wallet.md).



**Step 2. Send required data to reseller**

To make sure everything gets correctly registered, send the following information to your reseller and request to be updated to the new license system:

* Your GUNTHY wallet address
* A list of your currently registered API keys / master keys, including the names of the exchanges

After everything is registered, you'll receive the required number of tokens and you're ready to run Gunbot on the new license system.



**Step 3. Enter your GUNTHY wallet address** 

Enter your GUNTHY wallet address on **Settings &gt; Prefences &gt; Gunthy Wallet**.

![](../../../.gitbook/assets/image%20%2847%29.png)

Make sure that all of your master keys are set on the [Swap Exchanges](../connect-exchange/api-slots.md) page. Anytime you need to update a key, you can do it yourself using this page.

## FAQ

**Are there costs involved when I update my API key?**

No, when you manage your own API keys there are no costs for changing them.

**How often can I update my API key?**

You can update your API keys as often as you want.

**Can I run multiple Gunbot instances using the same GUNTHY wallet?**

Yes.

**I have more than one registered API key for the same exchange, what to do?**

If you have multiple registered API keys for the same exchange, each key needs to be registered to a different GUNTHY wallet. You must run multiple Gunbot instances to do so.

**Can I change my API key to another exchange?**

Yes, you can change to any supported exchange at any time.

**Is it mandatory to register a GUNTHY wallet for my API keys?**

Yes.

**Can't I just use any wallet address?**

Abuse will not be tolerated. Only use your own GUNTHY wallet address in Gunbot, not a wallet provided by an exchange. Using your own wallet address is the only way to receive and control the required amount of tokens.

