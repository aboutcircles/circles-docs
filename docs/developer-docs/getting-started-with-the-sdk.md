---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
icon: rectangles-mixed
---

# Quickstart Guide for Circles SDK

**Prerequisites**

* Browser wallet such as Metamask or Rabby Wallet
* Setup or add [Gnosis Chain (Mainnet)](https://docs.gnosischain.com/about/networks/mainnet) as your **network for wallet**. Check out Gnosis Chain docs [here](https://docs.gnosischain.com/about/networks/).
* Some xDAI token for paying gas fees. Check out [Mainnet](https://faucet.gnosischain.com/) Faucet

## Circles SDK Installation

If you have all prerequisites in place, start by installing the Circles SDK package and ethers v6 in your project using `npm.`

{% code overflow="wrap" %}
```
npm i @circles-sdk/sdk @circles-sdk/data @circles-sdk/utils @circles-sdk/profiles @circles-sdk/adapter-ethers ethers
```
{% endcode %}

## 1. Add imports

Then, import the necessary interfaces from the Circles SDK and Ethers.

```typescript
import { Sdk } from '@circles-sdk/sdk';
import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"
```

* `circlesConfig`: Contains predefined configurations, such as contract addresses for different environments (production version).
* `Sdk`: The main Circles SDK class, which provides methods to interact with the protocol, including token transfers, trust relationships, and contract interactions.

Since we are using browser-based wallets like Rabby and MetaMask, we will import the `BrowserProviderContractRunner` adapter, which connects the Circles SDK with Ethers.js, enabling smart contract interactions through a browser-based Ethereum provider.

## **2. Using Circles SDK Configuration**

Circles contracts are deployed on the Gnosis Chain mainnet. The following configuration is intended for production-ready applications.&#x20;

{% hint style="info" %}
If you are building dApps on Circles SDK for hackathon or want to check sandbox deployments, then follow [this segment](getting-started-with-the-sdk.md#using-the-sandbox-version-of-circles-sdk-for-local-development) to setup sandbox configuration for using the SDK.&#x20;
{% endhint %}

You can seamlessly import the production-ready smart contract addresses directly from the Circles SDK, as they are already hardcoded within the SDK.

```typescript
import { circlesConfig } from '@circles-sdk/sdk';
```

The `circlesConfig` object provides predefined contract addresses and service endpoints required for seamless integration with Circles on Gnosis Chain. It adheres to the `CirclesConfig` type from the Circles SDK.

<table><thead><tr><th width="247.1796875">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>v2PathfinderUrl</code></td><td>The URL for the V2 Pathfinder service (if using V2).</td></tr><tr><td><code>pathfinderUrl</code></td><td>The URL for the Pathfinder service (used in V1).</td></tr><tr><td><code>circlesRpcUrl</code></td><td>The URL for the Circles RPC service</td></tr><tr><td><code>profileServiceUrl</code></td><td>The URL for the profile service that manages user profiles in Circles.</td></tr><tr><td><code>v1HubAddress</code></td><td>The contract address for the Circles V1 Hub.</td></tr><tr><td><code>v2HubAddress?</code></td><td>The contract address for the Circles V2 Hub.</td></tr><tr><td><code>nameRegistryAddress</code></td><td>The address of the name registry contract.</td></tr><tr><td><code>migrationAddress</code></td><td>The address used for migrating avatars and tokens from V1 to V2.</td></tr><tr><td><code>baseGroupMintPolicy</code></td><td>The address of the minting policy used for group avatars in Circles.</td></tr><tr><td><code>coreMemberGroupDeployer</code></td><td>The address for groups that are deployed using core members instance</td></tr><tr><td><code>baseGroupFactory</code></td><td>The address for factory contract that will be used for groups deployment</td></tr></tbody></table>

#### Here's the configuration if you use `CirclesConfig` type from Circles SDK along with deployed contracts:

<details>

<summary>Configuration for Primary contracts</summary>

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const GnosisChainConfig: CirclesConfig = {
    circlesRpcUrl: "https://rpc.aboutcircles.com/",
    pathfinderUrl: "https://pathfinder.aboutcircles.com",
    profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
    v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",
    nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",
    v1NameRegistryAddress: "0x1eaD7F904F6fFC619c58B85e04F890b394E08172",
    migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",
    baseGroupMintPolicy: "0xcCa27c26CF7BAC2a9928f42201d48220F0e3a549",
    standardTreasury: "0x08F90aB73A515308f03A718257ff9887ED330C6e",
    coreMembersGroupDeployer: "0xFEca40Eb02FB1f4F5F795fC7a03c1A27819B1Ded",
    baseGroupFactory: "0xD0B5Bd9962197BEaC4cbA24244ec3587f19Bd06d"
    };
```

</details>

### Using the sandbox version of Circles SDK for builders

RINGS is a sandbox version of the Circles protocol designed for testing the Circles SDK without worrying about production requirements. If you are a builder or developer working on Circles for a hackathon and want to understand how the overall mechanics function, this is the place to start.

The only change you need to make to your codebase is updating the Circles configuration addresses to the sandbox contract addresses.

<details>

<summary>Configuration for RINGS contracts</summary>

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const circlesConfig: CirclesConfig = {
    circlesRpcUrl: 'https://static.94.138.251.148.clients.your-server.de/rpc/',
    pathfinderUrl: 'https://pathfinder.aboutcircles.com',
    profileServiceUrl: 'https://static.94.138.251.148.clients.your-server.de/profiles/',
    v1HubAddress: '0x29b9a7fbb8995b2423a71cc17cf9810798f6c543',
    v2HubAddress: '0x3D61f0A272eC69d65F5CFF097212079aaFDe8267',
    nameRegistryAddress: '0x8D1BEBbf5b8DFCef0F7E2039e4106A76Cb66f968',
    migrationAddress: '0x28141b6743c8569Ad8B20Ac09046Ba26F9Fb1c90',
    baseGroupMintPolicy: '0x79Cbc9C7077dF161b92a745345A6Ade3fC626A60',
    standardTreasury: '0x3545955Bc3900bda704261e4991f239BBd99ecE5',
    coreMembersGroupDeployer: '0x7aD59c08A065738e34f13Ac94542867528a1D328',
    baseGroupFactory:'0x452C116060cBB484eeDD70F32F08aD4F0685B5D2'
};
```

</details>

Once you have configured the sandbox addresses in your `config.ts` ,you can proceed with setting up the provider and signer as mentioned below.

## 3. Setup Provider and Signer

To setup provider and signer, we would utilize the Circles Adapter that is built to support transactions via ethers. Once you have already imported the `BrowserProviderContractRunner` , you would need to initialize it.

```typescript
const adapter = new BrowserProviderContractRunner();
await adapter.init();
```

## 4. Initialize the Circles SDK

To initialize the CirclesSDK, we will pass on the `Adapter` to SDK instance.

```typescript
const sdk = new Sdk (adapter); 
```

We cn also optionally pass circlesConfig manually, but if we don't it will get inferred automatically.

Once you have successfully created a SDK instance, you are all set to use Circles in your dApp.

## Summary for the setup of Circles SDK

To quickly summarize how the entire setup of Circles SDK would look like if you are writing a script etc.

#### Installation

{% code overflow="wrap" %}
```
npm i @circles-sdk/sdk @circles-sdk/data @circles-sdk/utils @circles-sdk/profiles @circles-sdk/adapter-ethers ethers
```
{% endcode %}

#### Import the packages and setup script

```typescript
import { circlesConfig, Sdk } from '@circles-sdk/sdk';
// or use the type import and then set up Circles contracts in configurations
// import type {CirclesConfig} from "@circles-sdk/sdk";
import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"

const adapter = new BrowserProviderContractRunner();
await adapter.init();

const sdk = new Sdk (adapter,circlesConfig); 
// or GnosisChainConfig as named in config
```

<details>

<summary>Optional : Setup deployed contracts based on production or sandbox version of Circles Protocol</summary>

**Production version:**

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const GnosisChainConfig: CirclesConfig = {
circlesRpcUrl: "https://rpc.aboutcircles.com/",
pathfinderUrl: "https://pathfinder.aboutcircles.com",
v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",
nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",
migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",
profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",
};
```

**Sandbox version:**&#x20;

```typescript
// For sandbox version
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



</details>

Voila :tada: you're done with Circles SDK setup. Now, you can support Circles profiles and choose the Avatars that you would like to integrate and support their additional functions.

## Choose which Avatar would you like to build on

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Personal / Human Avatars</strong></td><td>ERC-1155 standard avatars, which allows you to mint your personal Circles token (CRC) every hour, accumulating 24 CRC per day with an applied demurrage of 7%.</td><td></td></tr><tr><td><strong>Group Avatars</strong></td><td>Created by an owner, these avatars allow groups to trust human avatars within the group. Group tokens are utilized by collateralizing personal tokens, following the ERC-1155 standard.</td><td></td></tr><tr><td><strong>Organization Avatars</strong></td><td>As an organization, you are an avatar without any minting of new tokens. With your name and metadata file, which will be used for identification and can trust other avatars to receive Circles, with all owned Circles earned by avatars rather than minted.</td><td></td></tr></tbody></table>

***
