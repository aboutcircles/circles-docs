---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
icon: rectangles-mixed
---

# Quickstart Guide

**Prerequisites**

* A browser wallet such as MetaMask or Rabby Wallet.
* Set up or add [Gnosis Chain (Mainnet)](https://docs.gnosischain.com/about/networks/mainnet) as your wallet's network. Check out the Gnosis Chain docs [here](https://docs.gnosischain.com/about/networks/).
* Some xDAI tokens on Gnosis Chain for paying gas fees. Check out the [Mainnet Faucet](https://faucet.gnosischain.com/).

## 1. Installation

Once you have all prerequisites in place, start by installing the necessary Circles SDK packages and ethers v6 in your project using `npm`:

{% code overflow="wrap" %}
```bash
npm i @circles-sdk/sdk @circles-sdk/data @circles-sdk/utils @circles-sdk/profiles @circles-sdk/adapter-ethers ethers
```
{% endcode %}

## 2. Imports

Import the necessary components from the Circles SDK and Ethers:

```typescript
import { Sdk, type CirclesConfig } from '@circles-sdk/sdk';
import { BrowserProviderContractRunner } from "@circles-sdk/adapter-ethers";
```

*   `Sdk`: The main Circles SDK class, providing methods to interact with the protocol (token transfers, trust, etc.).
*   `CirclesConfig`: The type definition for the SDK configuration object.
*   `BrowserProviderContractRunner`: An adapter connecting the SDK with Ethers.js for browser-based wallet interactions (like MetaMask).

## 3. Configuration

The SDK needs a configuration object (`CirclesConfig`) specifying contract addresses and service endpoints. Provide an object following this structure when initializing the SDK:

<table><thead><tr><th width="247.1796875">Property</th><th>Description</th></tr></thead><tbody><tr><td><code>circlesRpcUrl</code></td><td>URL for the Circles RPC service.</td></tr><tr><td><code>profileServiceUrl</code></td><td>URL for the profile service managing user profiles.</td></tr><tr><td><code>v1HubAddress</code></td><td>Contract address for the Circles V1 Hub.</td></tr><tr><td><code>v2HubAddress?</code></td><td>Contract address for the Circles V2 Hub (optional if only using V1).</td></tr><tr><td><code>pathfinderUrl?</code></td><td>URL for the V1 Pathfinder service (optional if only using V2).</td></tr><tr><td><code>v2PathfinderUrl?</code></td><td>URL for the V2 Pathfinder service (optional if only using V1).</td></tr><tr><td><code>nameRegistryAddress</code></td><td>Address of the name registry contract.</td></tr><tr><td><code>migrationAddress</code></td><td>Address for migrating avatars/tokens from V1 to V2.</td></tr><tr><td><code>standardTreasury?</code></td><td>Address of the standard treasury contract (V2).</td></tr><tr><td><code>baseGroupMintPolicy?</code></td><td>Address of the base minting policy for group avatars (V2).</td></tr><tr><td><code>coreMembersGroupDeployer?</code></td><td>Address of the deployer for core member groups (V2, description TBC).</td></tr></tbody></table>

Choose the configuration for your target environment:

#### Gnosis Chain (Production)

Use this configuration for live applications on the Gnosis Chain mainnet. You can import a pre-filled configuration object directly from the SDK:

```typescript
import { circlesConfig } from '@circles-sdk/sdk';

// circlesConfig contains the production addresses and endpoints.
// You can also define it manually if needed:
/*
import type {CirclesConfig} from "@circles-sdk/sdk";

export const GnosisChainConfig: CirclesConfig = {
    circlesRpcUrl: "https://rpc.aboutcircles.com/",
    pathfinderUrl: "https://pathfinder.aboutcircles.com", // V1 Pathfinder
    v2PathfinderUrl: "https://pathfinder-v2.aboutcircles.com", // V2 Pathfinder (Example URL, verify correct one)
    profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
    v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",
    nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",
    migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",
    standardTreasury: "0x08F90aB73A515308f03A718257ff9887ED330C6e",
    baseGroupMintPolicy: "0xcCa27c26CF7BAC2a9928f42201d48220F0e3a549",
    coreMembersGroupDeployer: "0xFEca40Eb02FB1f4F5F795fC7a03c1A27819B1Ded"
};
*/
```

#### RINGS (Sandbox)

RINGS is the sandbox environment for testing and development. Use these addresses if you're building for fun, for a hackathon, or to understand the mechanics without using real funds.

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const ringsConfig: CirclesConfig = {
    circlesRpcUrl: "https://static.94.138.251.148.clients.your-server.de/rpc/",
    // pathfinderUrl: "...", // Add V1 Pathfinder URL for RINGS if available
    // v2PathfinderUrl: "...", // Add V2 Pathfinder URL for RINGS if available
    profileServiceUrl: "https://static.94.138.251.148.clients.your-server.de/profiles/",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543", // Verify if RINGS uses the same V1 Hub
    v2HubAddress: "0x3D61f0A272eC69d65F5CFF097212079aaFDe8267",
    nameRegistryAddress: "0x8D1BEBbf5b8DFCef0F7E2039e4106A76Cb66f968",
    migrationAddress: "0x28141b6743c8569Ad8B20Ac09046Ba26F9Fb1c90",
    // standardTreasury: "...", // Add RINGS Standard Treasury address if available
    baseGroupMintPolicy: "0x79Cbc9C7077dF161b92a745345A6Ade3fC626A60",
    // coreMembersGroupDeployer: "..." // Add RINGS Core Members Group Deployer address if available
};
```
{% hint style="warning" %}
Note: Some RINGS addresses (like Pathfinder, Treasury, V1 Hub) might differ from the example above or may not be available. Verify the correct endpoints and addresses for the RINGS environment.
{% endhint %}

## 4. Set Up Provider and Signer Adapter

To interact with the blockchain via the user's browser wallet, set up the Ethers adapter:

```typescript
const adapter = new BrowserProviderContractRunner();
await adapter.init(); // Connects to the browser wallet (e.g., MetaMask)
```

## 5. Initialize the Circles SDK

Instantiate the SDK, passing the adapter and the chosen configuration (`circlesConfig` for production or `ringsConfig` for sandbox):

```typescript
const sdk = new Sdk(adapter, circlesConfig); // Use circlesConfig for Production
// const sdk = new Sdk(adapter, ringsConfig); // Use ringsConfig for Sandbox
```

Once the SDK instance is created, you're ready to use Circles in your dApp!

## Summary of Setup

Here's a concise summary of the setup process:

1.  **Install Packages:**
    {% code overflow="wrap" %}
    ```bash
    npm i @circles-sdk/sdk @circles-sdk/data @circles-sdk/utils @circles-sdk/profiles @circles-sdk/adapter-ethers ethers
    ```
    {% endcode %}
2.  **Import and Configure:**
    ```typescript
    import { Sdk, circlesConfig, type CirclesConfig } from '@circles-sdk/sdk'; // circlesConfig is for Production
    import { BrowserProviderContractRunner } from "@circles-sdk/adapter-ethers";

    // Define ringsConfig if using Sandbox (see Step 3 above)
    // const ringsConfig: CirclesConfig = { ... };

    // Choose config:
    const chosenConfig = circlesConfig; // Or ringsConfig

    // Setup Adapter
    const adapter = new BrowserProviderContractRunner();
    await adapter.init();

    // Initialize SDK
    const sdk = new Sdk(adapter, chosenConfig);

    // Now use the 'sdk' instance...
    ```

You're done with the Circles SDK setup! Now, you can integrate Circles profiles and choose which Avatars to support.

## Choose Which Avatar to Build On

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Personal (Human) Avatars</strong></td><td>ERC-1155 standard avatars allowing minting of personal Circles tokens (CRC) every hour (24 CRC/day), subject to 7% annual demurrage.</td><td></td></tr><tr><td><strong>Group Avatars</strong></td><td>Created by an owner, allowing groups to be trusted by human avatars. Group tokens are created by collateralizing personal tokens, following the ERC-1155 standard.</td><td></td></tr><tr><td><strong>Organization Avatars</strong></td><td>An avatar type without the ability to mint new tokens. Uses a name and metadata for identification. Can trust other avatars to receive Circles; all owned Circles must be earned or received, not minted. Subject to demurrage.</td><td></td></tr></tbody></table>
