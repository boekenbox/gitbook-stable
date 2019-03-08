# Register GUNTHY wallet

Gunbot uses a blockchain based license validation system. It uses our own ERC-20 utility token called "GUNTHY". Using this system, you can manage your own API keys and change them anytime, or even sell your license to third party.

_Your wallet address, combined with an airdropped amount of tokens, is your Gunbot license key._

{% hint style="warning" %}
 **You are responsible for your own API keys and GUNTHY wallet.** 

Please understand that we might not be able to help you if you lose all your registered API keys and \(access to\) your wallet address. Make sure to follow all the security advise when setting up a wallet and properly save and backup the API key\(s\) used for Gunbot.
{% endhint %}



## Registering API keys to a GUNTHY wallet address

{% hint style="success" %}
New users register the wallet address and API keys at time of purchase. 

In that case only the wallet address and API keys need to be entered in Gunbot. 
{% endhint %}

After your reseller registered an [API key](https://github.com/GuntharDeNiro/BTCT/wiki/api) to be used for Gunbot, you can take control and update it yourself. 



**Step 1: Reseller registers your API key to be used with Gunbot.**

For every Gunbot license, one exchange API needs to be registered to a GUNTHY wallet address. If you license allows for multiple exchange slots, you can register the others yourself anytime.



**Step 2. Create a GUNTHY wallet.**

GUNTHY is an ERC-20 token, this means that most Ethereum wallets are compatible. You need to use a wallet you own, a wallet address at an exchange will not work.

Use this smart contract to add the GUNTHY token to your wallet: [https://etherscan.io/address/0x3684b581db1f94b721ee0022624329feb16ab653](https://etherscan.io/address/0x3684b581db1f94b721ee0022624329feb16ab653)

Contract address: `0x3684b581db1f94b721ee0022624329feb16ab653`

If your wallet software asks about the number of decimals to use for GUNTHY, enter 18.

Read more about the [steps to create a GUNTHY wallet](https://github.com/GuntharDeNiro/BTCT/wiki/api-key-management#steps-to-create-a-gunthy-wallet).



**Step 3a. Enter your exchange API key\(s\)**

Enter your exchange key\(s\) on **Settings &gt; Trading &gt; Exchanges**.

In Gunbot, use your registered API key as master key. Optionally you can use a secondary API key for trading, this key needs to exist in the same exchange account as the registered API.

![](../../../.gitbook/assets/image%20%2815%29.png)

\*\*\*\*

**Step 3b. Register your GUNTHY wallet address**

Enter your GUNTHY wallet address on **Settings &gt; Prefences &gt; Gunthy Wallet**.

Enter **your** GUNTHY wallet address. All of your API keys are then registered to this wallet address.

When you add registered keys to Gunbot after already having set your wallet address, the new keys are automatically registered to this wallet.

![](../../../.gitbook/assets/image%20%288%29.png)

\*\*\*\*

**Step 4. Receive GUNTHY airdrop**

After you've registered your wallet address, you'll receive an airdrop with the required amount of tokens for your license type. **Hold these tokens at all times to be sure your Gunbot can run.**

These are the required amounts per license:

* **Starter**: 400 GUNTHY
* **Standard**: 1000 GUNTHY
* **Pro**: 1500 GUNTHY
* **Ultimate**: 2500 GUNTHY

> During the initial roll out of the new license system, airdrops are not yet automatic. Gunbot will work without a token balance until this process is fully tried and tested.



## Updating GUNTHY wallet

If you need to switch to another GUNTHY wallet, you can update it on **Settings &gt; Prefences &gt; Gunthy Wallet**.

All of your registered API keys connected to the old wallet address will be registered to the new wallet address after updating it.

![](../../../.gitbook/assets/image%20%285%29.png)



