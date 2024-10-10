---
icon: folder-tree
description: An overview of different components of circles architecture.
---

# Circles Architecture

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>A flow of architecture for Circles Protocol</p></figcaption></figure>



## Circles V2 Core Components

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><mark style="color:blue;"><strong>Hub V2 Contract</strong></mark></td><td><p>An ERC-1155 standard contract for registeration of </p><ul><li>human,</li><li>groups and </li><li>organisation avatars. </li></ul><p>Manages trust relations, minting of personal CRC tokens, group currencies and demurrage.</p></td></tr><tr><td><mark style="color:blue;"><strong>Migration Contract</strong></mark></td><td>Allows transition from Legacy V1 hub avatars to V2 hub. Migration will lock V1 CRC tokens, stop minting V1 tokens and convert into V2 tokens.</td></tr><tr><td><mark style="color:blue;"><strong>Name Registry</strong></mark></td><td><p>NameRegistry contract manages names, symbols and metadata for avatars (humans, groups, and organizations).</p><p>The name would be of 12 characters with a base58 encoding and store metadata for avatar profiles.</p></td></tr><tr><td><mark style="color:blue;"><strong>Base Mint Policy</strong></mark></td><td>Base mint policy is standard contract is utilized group registration. Once registered the policy address is immutable for the group address. This is a reference implementation for minting, burning and redeeming the group currencies and developers can build their own custom policies as well.</td></tr><tr><td><mark style="color:blue;"><strong>Vaults</strong></mark></td><td>Vaults is a factor contract that holds the personal CRC collateral against the group currencies. Every group, there is single vault to query balance. This contract is deployed by Standard treasury and is utilized during redemption of group circles token.</td></tr><tr><td><mark style="color:blue;"><strong>Standard Treasury</strong></mark></td><td>The Standard Treasury handles minting and redemption of group Circles by managing collateral transfers. It ensures collateral is forwarded to the correct vault based on structured data from the Hub contract. Additionally, it verifies data during redemption to release or burn collateral as specified by the group's mint policy.</td></tr></tbody></table>

