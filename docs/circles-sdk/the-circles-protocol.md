---
description: Developers Hub for building with Circles.
icon: layer-group
---

# The Circles Stack

Our developer documentation portal provides comprehensive guide to build using Circles SDK, SDK references, contract addresses, and code examples to help you integrate Circles into your applications and build upon our ecosystem.

## Circles SDK

If you want to develop a server or client application that utilizes Circles, and allow you to utilize trust connection and personal/group currencies, then Circles SDK would be your entry point. Based on your need, you can pick to develop any avatar post initialization of SDK.

* [Circles SDK package on npm](https://www.npmjs.com/package/@circles-sdk/sdk)
* [Circles SDK source code](https://github.com/aboutcircles/sdk)

## Circles Infrastructure

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Pathfinder</strong></td><td><p>Finds liquid paths between two accounts in the trust network. These paths are used as input for the contract's transfer methods.<br></p><p><a href="https://github.com/aboutcircles/circles-nethermind-plugin/tree/dev/Circles.Pathfinder">Learn more about Pathfinder</a></p></td><td></td></tr><tr><td><strong>Circles Nethermind Plug-in</strong></td><td><p>Provides access to the Gnosis Chain and indexes Circles events for a seamless experience.<br></p><p><a href="https://github.com/aboutcircles/circles-nethermind-plugin">Learn more about Circles Nethermind Plug-in</a></p></td><td></td></tr></tbody></table>

## Circles Hub Contracts

Circles protocol relies on the hub contract that you can utilize directly within applications. These deployed contracts exist on Gnosis Chain and are required by the SDK for configuration and initialization.

Here are the deployed addresses for V1 hub and V2 hub that you should consider for configuration or building tools on Circles protocol:

<table><thead><tr><th width="192.9765625">Contract Name</th><th width="563.39453125">Deployed addresses</th></tr></thead><tbody><tr><td>Hub contract</td><td>0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8</td></tr><tr><td>Name registry</td><td>0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474</td></tr><tr><td>Migration contract</td><td>0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376</td></tr><tr><td>Base mint policy</td><td>0xcCa27c26CF7BAC2a9928f42201d48220F0e3a549</td></tr></tbody></table>

If you want to skip directly to setting up Circles SDK in your application, you can jump to the SDK Configuration Guide which covers all the setup parameters.

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

<table data-view="cards"><thead><tr><th></th><th></th><th data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Circles V2 contract source code</strong></td><td>Review the codebase for Circles contracts V2 which follows ERC1155 standard, and manages personal, group and organisation avatars.</td><td><a href="https://github.com/aboutcircles/circles-contracts-v2">https://github.com/aboutcircles/circles-contracts-v2</a></td></tr><tr><td><strong>Circles V2 Reference docs</strong></td><td>Explore the latest updates and functionalities of Circles v2.0 Contracts with detailed documentation.</td><td><a href="https://aboutcircles.github.io/circles-contracts-v2/">https://aboutcircles.github.io/circles-contracts-v2/</a></td></tr><tr><td>Circles Profile Service</td><td>This service indexes and persists profile data from/to IPFS.</td><td><a href="https://github.com/aboutcircles/profile-service">https://github.com/aboutcircles/profile-service</a></td></tr></tbody></table>

## Join the Technical Community

To collaborate with fellow developers, ask questions, and share your insights, join our technical community channels on Discord, GitHub, and other platforms.

Join [Telegram Group ](https://t.me/about_circles/1)as a Hacker or a Developer.
