---
icon: user-hair
---

# Personal Currencies

Circles is all about creating a fairer money system. Our current monetary system and most cryptocurrencies tend to benefit those with established wealth and market positions, making it challenging for newcomers to catch up. Wealth grows disproportionately for those already in the market, as time in the market often beats time to market.

Circles aims to solve this by ensuring equal opportunity for all. Each user continuously generates tokens at a rate of one Circle (CRC) per hour, regardless of when they join. Additionally, existing Circles incur a demurrage fee of 7% per year. By tying the issuance of tokens to time, a resource everyone has, Circles promotes fairness by giving everyone a chance to accumulate tokens and encouraging the active circulation of currency.

### **Issuance**

Circles employs a unique token issuance mechanism where each user generates their own currency (CRC) at a steady rate of one token per hour. The minting rules are encoded in the so-called Hub, a token factory from which all individual Circles tokens are created.&#x20;

{% hint style="info" %}
All tokens existing in the system are ultimately rooted in personal tokens (Circles v1 only includes personal tokens, while v2 adds group currencies which are collateralized by personal tokens).
{% endhint %}

### **Demurrage**

All tokens are automatically demurred at a rate of 7% per year.

For human users, the demurrage is offset by the steady income of new CRC. Only after minting for 80 years (roughly a human lifespan) or through economic activity can a person become negatively affected by demurrage. Once the person dies, no new tokens are minted and all of the person's tokens are subjected to demurrage.

{% hint style="info" %}
Organizations cannot mint tokens. Therefore, their balance is always subject to a demurrage of 7% per year.
{% endhint %}

The following graph visualizes the balance of an account over time, assuming no economic activity and continuous minting of all CRC.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

If understood as a tax, then the tax would be negative at first (you get money), but once your balance reaches the threshold of 125.000 Circles, it turns positive (you loose money).

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### **Implementation**

Circles v1 tokens adhere to the ERC20 token standard with added functionality for personal minting and a built-in allowance like mechanism for the Hub contract which is necessary to enable the path transfer functionality.

Circles v2 is built on the ERC1155 token standard. Here, the Hub uses a standard allowance to facilitate path transfers. The token metadata mimics as profile.

### Dead man's switch

Circles v1 tokens have a built-in "dead man's switch" that permanently disables the minting of new tokens after 90 days of inactivity. This proved problematic, so in Circles v2, the mechanism was replaced by a limit that allows a maximum minting amount of 14 days' worth of Circles.&#x20;

In both versions, minting can be stopped manually. This feature is used, for example, to ensure that users who migrate from v1 to v2 cannot mint in v1 before they are allowed to mint in v2.

