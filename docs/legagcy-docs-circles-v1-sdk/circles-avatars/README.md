---
description: An avatar represents a Circles user.
icon: circle-user
---

# Circles Avatars

The SDK is built around the concept of avatars. An avatar is a Circles user and is used to interact with other Avatars.&#x20;

* New Circles users must sign up.
* For existing Circles users, you can simply get exisiting avatars by address.

### Creating a new avatar

{% hint style="info" %}
You will need a Metamask account with some xDAI to be able to interact with contracts and follow along the steps. Make sure you configured MetaMask with the correct ChainID (Gnosis Chain or Chiado) before you continue.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Personal / Human Avatars</strong></td><td>ERC-1155 standard avatars, which allows you to mint your personal Circles token (CRC) every hour, accumulating 24 CRC per day with an applied demurrage of 7%.</td><td></td><td><a href="personal-human-avatars/">personal-human-avatars</a></td></tr><tr><td><strong>Group Avatars</strong></td><td>Created by an owner, these avatars allow groups to trust human avatars within the group. Group tokens are utilized by collateralizing personal tokens, following the ERC-1155 standard.</td><td></td><td><a href="group-avatars/">group-avatars</a></td></tr><tr><td><strong>Organization Avatars</strong></td><td>As an organization, you are an avatar without any minting of new tokens. With your name and metadata file, which will be used for identification and can trust other avatars to receive Circles, with all owned Circles earned by avatars rather than minted.</td><td></td><td><a href="organization-avatars/">organization-avatars</a></td></tr></tbody></table>
