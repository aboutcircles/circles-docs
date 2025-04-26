---
description: An avatar represents a Circles user.
icon: circle-user
---

# Circles Avatars

The SDK is built around the concept of avatars. An avatar is a Circles user and is used to interact with other Avatars.&#x20;

* New Circles users must sign up.
* For existing Circles users, you can simply get existing avatars by address.

### Creating a New Avatar

{% hint style="info" %}
You will need a wallet like MetaMask with some xDAI on Gnosis Chain to interact with contracts and follow the steps. Make sure you have configured your wallet with the correct ChainID (Gnosis Chain mainnet: 100) before you continue.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Personal (Human) Avatars</strong></td><td>ERC-1155 standard avatars allowing minting of personal Circles tokens (CRC) every hour (24 CRC/day), subject to 7% annual demurrage.</td><td></td><td><a href="personal-human-avatars/">personal-human-avatars</a></td></tr><tr><td><strong>Group Avatars</strong></td><td>Created by an owner, allowing groups to be trusted by human avatars. Group tokens are created by collateralizing personal tokens, following the ERC-1155 standard.</td><td></td><td><a href="group-avatars/">group-avatars</a></td></tr><tr><td><strong>Organization Avatars</strong></td><td>An avatar type without the ability to mint new tokens. Uses a name and metadata for identification. Can trust other avatars to receive Circles; all owned Circles must be earned or received, not minted. Subject to demurrage.</td><td></td><td><a href="organization-avatars/">organization-avatars</a></td></tr></tbody></table>
