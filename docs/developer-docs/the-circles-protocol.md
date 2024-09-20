---
description: Developers Hub for building with Circles.
---

# 🪜 The Circles Stack

## Building on the Circles protocol

Our developer documentation portal provides comprehensive guides, API references, and code examples to help you integrate Circles into your applications and build upon our ecosystem.

### Contracts

Depending on what you want to achieve, you'll might want to use the contracts directly from solidity. In this case you can check out the source code on GitHub. For Circles v2 there is an a reference available.

* [Circles v1 contract source](https://github.com/circlesubi/circles-contracts/)
* [Circles v2 contract source (v0.3.6)](https://github.com/aboutcircles/circles-contracts-v2/releases/tag/rc-v0.3.6-alpha)
* Circles v2 reference docs

#### Deployments

{% tabs %}
{% tab title="Gnosis chain" %}
Hub address (v1):

[`0x29b9a7fbb8995b2423a71cc17cf9810798f6c543`](https://gnosisscan.io/address/0x29b9a7fbb8995b2423a71cc17cf9810798f6c543)
{% endtab %}

{% tab title="Chiado (v0.3.6)" %}
Hub address (v1):

[`0xdbf22d4e8962db3b2f1d9ff55be728a887e47710`](https://gnosis-chiado.blockscout.com/address/0xdbF22D4e8962Db3b2F1d9Ff55be728A887e47710)

Hub address (v2 v0.3.6):

[`0xa5c7ADAE2fd3844f12D52266Cb7926f8649869Da`](https://gnosis-chiado.blockscout.com/address/0xa5c7ADAE2fd3844f12D52266Cb7926f8649869Da)

Migration contract (v2 v0.3.6):

[0xe1dCE89512bE1AeDf94faAb7115A1Ba6AEff4201](https://gnosis-chiado.blockscout.com/address/0xe1dCE89512bE1AeDf94faAb7115A1Ba6AEff4201)
{% endtab %}
{% endtabs %}

### SDK

If you want to develop a server or client application that utilizes Circles, you can check out the SDK and just keep reading (This documentation pages are mostly about the SDK).

* [Circles SDK source](https://github.com/aboutcircles/circles-sdk)
* [Circles SDK package on npm](https://www.npmjs.com/package/@circles-sdk/sdk)

The SDK allows you to seamlessly use all features of Circles v1 and Circles v2. These include:

* Signing up as a human, organisation, or group
* Trusting, minting, and transferring personal and group Circles
* Querying balances, trust relations, and transaction histories
* Subscribing to and querying events

### Infrastructure

Circles relies on some infrastructure components to make it work. Most importantly:

* [Pathfinder](https://github.com/circlesUBI/pathfinder2) - Finds liquid paths between two accounts in the trust network. These are used as input for the contract's transfer methods.
* [Circles Nethermind plug-in](https://github.com/aboutcircles/circles-nethermind-plugin) - Provides access to the Gnosis Chain and indexes Circles events.

## Join the Technical Community

To collaborate with fellow developers, ask questions, and share your insights, join our technical community channels on Discord, GitHub, and other platforms.

Join [Telegram Group ](https://t.me/about\_circles/1)as Hacker or Developer.
