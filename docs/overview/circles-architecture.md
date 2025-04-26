---
icon: folder-tree
description: An overview of different components of circles architecture.
---

# Circles Architecture

<figure><img src="../.gitbook/assets/image (3).png" alt="Diagram illustrating the flow of Circles Protocol architecture"><figcaption><p>Architectural Flow of the Circles Protocol</p></figcaption></figure>



## Circles V2 Core Components

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><mark style="color:blue;"><strong>Hub V2 Contract</strong></mark></td><td><p>An ERC-1155 standard contract for the registration of </p><ul><li>human,</li><li>group, and </li><li>organization avatars. </li></ul><p>Manages trust relationships, minting of personal CRC tokens, group currencies, and demurrage.</p></td></tr><tr><td><mark style="color:blue;"><strong>Migration Contract</strong></mark></td><td>Allows transition from Legacy V1 hub avatars to the V2 hub. Migration locks V1 CRC tokens, stops the minting of V1 tokens, and converts them into V2 tokens.</td></tr><tr><td><mark style="color:blue;"><strong>Name Registry</strong></mark></td><td><p>The NameRegistry contract manages names, symbols, and metadata for avatars (humans, groups, and organizations).</p><p>The name is 12 characters long with base58 encoding and stores metadata for avatar profiles.</p></td></tr><tr><td><mark style="color:blue;"><strong>Base Mint Policy</strong></mark></td><td>The Base Mint Policy is a standard contract utilized during group registration. Once registered, the policy address is immutable for the group address. This is a reference implementation for minting, burning, and redeeming group currencies; developers can build their own custom policies as well.</td></tr><tr><td><mark style="color:blue;"><strong>Vaults</strong></mark></td><td>Vaults is a factory contract that holds the personal CRC collateral against group currencies. For every group, there is a single vault to query balance. This contract is deployed by the Standard Treasury and is utilized during the redemption of group Circles tokens.</td></tr><tr><td><mark style="color:blue;"><strong>Standard Treasury</strong></mark></td><td>The Standard Treasury handles the minting and redemption of group Circles tokens by managing collateral transfers. It ensures collateral is forwarded to the correct vault based on structured data received from the Hub contract. Additionally, it verifies data during the redemption process to release or burn collateral as specified by the group's mint policy.</td></tr></tbody></table>
