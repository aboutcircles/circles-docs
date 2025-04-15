---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
icon: rectangles-mixed
---

# Getting started with the SDK

## Prerequisites

* [Metamask Plugin](https://metamask.io/) installed in browser
* Setup [Gnosis Chain (Mainnet)](https://docs.gnosischain.com/about/networks/mainnet) and/or [Chiado Chain (Testnet)](https://docs.gnosischain.com/about/networks/chiado). Check out Gnosis Chain docs [here](https://docs.gnosischain.com/about/networks/).
* xDAI as gas token, check out [Mainnet](https://faucet.gnosischain.com/) and [Testnet ](https://faucet.chiadochain.net/)Faucet

## Install packages for CirclesSDK

If you have all prerequisites in place, start by installing the Circles SDK package and ethers v6 in your project using `npm.`

{% code overflow="wrap" %}
```
npm i @circles-sdk/sdk @circles-sdk/data @circles-sdk/utils @circles-sdk/profiles @circles-sdk/adapter-ethers ethers
```
{% endcode %}

### 1. Add imports

Then, import the necessary interfaces from the Circles SDK and Ethers.

```typescript
import { CirclesConfig, Sdk } from '@circles-sdk/sdk';
import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"
```

### 2. Add `CirclesConfig` for SDK

**`CirclesConfig`** defines the configuration settings needed to set up the SDK. You provide an object that follows this structure when initializing the SDK.

Circles is available on Gnosis Chain and Chiado Testnet. You need to specify the correct contract addresses and service endpoints for each environment.

<table><thead><tr><th width="247.1796875">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>v2PathfinderUrl</code></td><td>The URL for the V2 Pathfinder service (if using V2).</td></tr><tr><td><code>pathfinderUrl</code></td><td>The URL for the Pathfinder service (used in V1).</td></tr><tr><td><code>circlesRpcUrl</code></td><td>The URL for the Circles RPC service</td></tr><tr><td><code>profileServiceUrl</code></td><td>The URL for the profile service that manages user profiles in Circles.</td></tr><tr><td><code>v1HubAddress</code></td><td>The contract address for the Circles V1 Hub.</td></tr><tr><td><code>v2HubAddress?</code></td><td>The contract address for the Circles V2 Hub.</td></tr><tr><td><code>nameRegistryAddress</code></td><td>The address of the name registry contract.</td></tr><tr><td><code>migrationAddress</code></td><td>The address used for migrating avatars and tokens from V1 to V2.</td></tr><tr><td><code>baseGroupMintPolicy</code></td><td>The address of the minting policy used for group avatars in Circles.</td></tr><tr><td><code>coreMemberGroupDeployer</code></td><td></td></tr></tbody></table>

{% tabs %}
{% tab title="Gnosis Chain " %}
Circles contracts are deployed on Gnosis Chain mainnet. The below config is for applications that would be built in production environment.

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const GnosisChainConfig: CirclesConfig = {
    circlesRpcUrl: "https://rpc.aboutcircles.com/",
    pathfinderUrl: "https://pathfinder.aboutcircles.com",
    profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
    v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",
    nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",
    migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",
    standardTreasury: "0x08F90aB73A515308f03A718257ff9887ED330C6e",
    baseGroupMintPolicy: "0xcCa27c26CF7BAC2a9928f42201d48220F0e3a549",
    coreMembersGroupDeployer: "0xFEca40Eb02FB1f4F5F795fC7a03c1A27819B1Ded"
};
```
{% endtab %}

{% tab title="RINGS - sandbox version of Circles" %}
RINGS is a sandbox version of Circles protocol. If you are a builder or developer who is building on Circles for fun and want to see how the overall mechanics work - then this would be your place to begin.

{% code overflow="wrap" %}
```typescript

import type {CirclesConfig} from "@circles-sdk/sdk";

export const circlesConfig: CirclesConfig = {
    circlesRpcUrl: "https://static.94.138.251.148.clients.your-server.de/rpc/",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
    v2HubAddress: "0x3D61f0A272eC69d65F5CFF097212079aaFDe8267",
    migrationAddress: "0x28141b6743c8569Ad8B20Ac09046Ba26F9Fb1c90",
    nameRegistryAddress: "0x8D1BEBbf5b8DFCef0F7E2039e4106A76Cb66f968",
    baseGroupMintPolicy: "0x79Cbc9C7077dF161b92a745345A6Ade3fC626A60",
    profileServiceUrl: "https://static.94.138.251.148.clients.your-server.de/profiles/",
};

```
{% endcode %}
{% endtab %}
{% endtabs %}

### 3. Setup Provider and Signer

To setup provider and signer, we would utilize the Circles Adapter that is built to support transactions via ethers. Once you have already imported the `BrowserProviderContractRunner` , you would need to initialize it.

```typescript
const adapter = new BrowserProviderContractRunner();
await adapter.init();
```

### 3. Initialize the Circles SDK

To initialize the CirclesSDK, we will pass on the `CirclesConfig` and `Adapter` to SDK instance.

```typescript
const sdk = new Sdk (adapter,CirclesConfig);
```

Once you have successfully created a SDK instance, you are all set to use Circles in your dApp. Let's learn more about the Circles SDK features and how you can use them on the next pages.\\

## Choose which Avatar would you like to build on

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Personal / Human Avatars</strong></td><td>ERC-1155 standard avatars, which allows you to mint your personal Circles token (CRC) every hour, accumulating 24 CRC per day with an applied demurrage of 7%.</td><td></td></tr><tr><td><strong>Group Avatars</strong></td><td>Created by an owner, these avatars allow groups to trust human avatars within the group. Group tokens are utilized by collateralizing personal tokens, following the ERC-1155 standard.</td><td></td></tr><tr><td><strong>Organization Avatars</strong></td><td>As an organization, you are an avatar without any minting of new tokens. With your name and metadata file, which will be used for identification and can trust other avatars to receive Circles, with all owned Circles earned by avatars rather than minted.</td><td></td></tr></tbody></table>

***
