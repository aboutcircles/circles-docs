---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
---

# 🚀 Getting started with the SDK

## Prerequisites

* [Metamask Plugin](https://metamask.io/) installed in browser
* Setup [Gnosis Chain (Mainnet)](https://docs.gnosischain.com/about/networks/mainnet) and/or [Chiado Chain (Testnet)](https://docs.gnosischain.com/about/networks/chiado). Check out Gnosis Chain docs [here](https://docs.gnosischain.com/about/networks/).
* xDAI as gas token, check out [Mainnet](https://faucet.gnosischain.com/) and [Testnet ](https://faucet.chiadochain.net/)Faucet

{% hint style="info" %}
_You should use Chiado if you want to test a preview version of Circles v2. If you want to build with Circles v1, use Gnosis Chain instead._
{% endhint %}

## Install packages for CirclesSDK

If you have all prerequisites in place, start by installing the Circles SDK package and ethers v6 in your project using `npm`

* `npm i ethers`
* `npm i @circles-sdk/sdk@0.7.1 @circles-sdk/adapter-ethers@0.7.1`&#x20;
* `npm i @circles-sdk/data@0.7.1 @circles-sdk/profiles@0.7.1`

### 1. Add imports

Then, import the necessary classes from the Circles SDK and Ethers.

```typescript
import { Sdk,ChainConfig } from "@circles-sdk/sdk";
import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"
```

### 2. Add `CirclesConfig` for SDK

Circles is available on Gnosis Chain and Chiado Testnet. You need to specify the correct contract addresses and service endpoints for each environment.&#x20;

{% tabs %}
{% tab title="Chiado" %}
```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const config: CirclesConfig = {
    circlesRpcUrl: "https://chiado-rpc.aboutcircles.com",
    pathfinderUrl: "https://chiado-pathfinder.aboutcircles.com",
    v2PathfinderUrl: "https://chiado-pathfinder.aboutcircles.com/pathfinder/",
    profileServiceUrl: "https://chiado-pathfinder.aboutcircles.com/profiles/",
    v1HubAddress: "0xdbf22d4e8962db3b2f1d9ff55be728a887e47710",
    v2HubAddress: "0xEddc960D3c78692BF38577054cb0a35114AE35e0",
    migrationAddress: "0x8C9BeAccb6b7DBd3AeffB5D77cab36b62Fe98882",
    nameRegistryAddress: "0x5525cbF9ad01a4E805ed1b40723D6377b336eCcf"
};
```
{% endtab %}

{% tab title="Gnosis Chain " %}
The Gnosis Chain mainnet is the production chain for Circles.&#x20;

```typescript
import type {CirclesConfig} from "@circles-sdk/sdk";

export const config: CirclesConfig = {
  pathfinderUrl: 'https://pathfinder.aboutcircles.com',
  circlesRpcUrl: 'https://rpc.helsinki.aboutcircles.com',
  v1HubAddress: '0x29b9a7fbb8995b2423a71cc17cf9810798f6c543',
  v2HubAddress: '0x7bC1F123089Bc1f384b6379d0587968d1CD5830a',
  migrationAddress: '0xEaBa6046103C3A2f5A681fD4323f78C647Fb4292',
  profileServiceUrl: 'https://chiado-pathfinder.aboutcircles.com/profiles/',
  nameRegistryAddress: "0xb95ef3f3e693531d9588815bca954dc8dce30937",
};
```
{% endtab %}
{% endtabs %}

### 3. Setup Provider and Signer

To setup provider and signer, we would utilize the Circles Adapter that is built to support transactions via ethers. Once you have already imported the `BrowserProviderContractRunner` , you would need to initialize it.

```typescript
const adapter = new BrowserProviderContractRunner();
await adapter.init();

const circlesAddress = adapter.address
```

### 3. Initialize the Circles SDK

To initialize the CirclesSDK, we will pass on the `ChainConfig` and `Adapter` to SDK instance.

```typescript
const sdk = new Sdk(chainConfig,adapter);
```

Once you have successfully created a SDK instance, you are all set to use Circles in your dApp. Let's learn more about the Circles SDK features and how you can use them on the next pages.

***

### CirclesSDK Code implementation for Svelte and React Framework

Here's the full code for Svelte and React for you to copy & paste:

{% tabs %}
{% tab title="Svelte" %}
```typescript
<script lang="ts">
    import { Sdk, type ChainConfig } from "@circles-sdk/sdk";
    import {BrowserProviderContractRunner} from "@circles-sdk/adapter-ethers"
    import { onMount } from "svelte";

    let avatarType = "";
    let circlesAddress = "";
    let sdk: Sdk;

    const chainConfig: ChainConfig = {
        circlesRpcUrl: "https://chiado-rpc.aboutcircles.com",
        pathfinderUrl: "https://chiado-pathfinder.aboutcircles.com",
        v2PathfinderUrl: "https://chiado-pathfinder.aboutcircles.com/pathfinder/",
        profileServiceUrl: "https://chiado-pathfinder.aboutcircles.com/profiles/",
        v1HubAddress: "0xdbf22d4e8962db3b2f1d9ff55be728a887e47710",
        v2HubAddress: "0xEddc960D3c78692BF38577054cb0a35114AE35e0",
        migrationAddress: "0x8C9BeAccb6b7DBd3AeffB5D77cab36b62Fe98882",
        nameRegistryAddress: "0x5525cbF9ad01a4E805ed1b40723D6377b336eCcf"
    };
    
   async function initializeSdk() {
        // Get Adapter
        const adapter = new BrowserProviderContractRunner();
        await adapter.init();

        // Get Circles Address
        circlesAddress = adapter.address;

        // Initialize the SDK with the adapter
        sdk = new Sdk(chainConfig, adapter);
    }

    // Fetch the avatar associated with the wallet address
    async function fetchAvatar() {
        if (sdk && circlesAddress) {
            const avatar = await sdk.getAvatar(circlesAddress);
            avatarType = avatar.avatarInfo?.type || "Unknown";
        }
    }

    // Mounting the component and initializing the SDK
    onMount(async () => {
        await initializeSdk();
        await fetchAvatar();
    });
</script>

<p>
    Avatar for {circlesAddress} is {avatarType}
</p>
```
{% endtab %}

{% tab title="React" %}
```tsx

import React, { useEffect, useState } from 'react';
import { Sdk, type ChainConfig } from "@circles-sdk/sdk";
import { BrowserProviderContractRunner } from "@circles-sdk/adapter-ethers";

const AvatarComponent = () => {
    const [circlesAddress, setCirclesAddress] = useState<string | undefined>(undefined);
    const [sdk, setSdk] = useState<Sdk | undefined>(undefined);
    const [avatarType, setAvatarType] = useState<string>("Unknown");
    const [avatarAddress, setAvatarAddress] = useState<string>("");

    const chainConfig: ChainConfig = {
        circlesRpcUrl: "https://chiado-rpc.aboutcircles.com",
        pathfinderUrl: "https://chiado-pathfinder.aboutcircles.com",
        v2PathfinderUrl: "https://chiado-pathfinder.aboutcircles.com/pathfinder/",
        profileServiceUrl: "https://chiado-pathfinder.aboutcircles.com/profiles/",
        v1HubAddress: "0xdbf22d4e8962db3b2f1d9ff55be728a887e47710",
        v2HubAddress: "0xEddc960D3c78692BF38577054cb0a35114AE35e0",
        migrationAddress: "0x8C9BeAccb6b7DBd3AeffB5D77cab36b62Fe98882",
        nameRegistryAddress: "0x5525cbF9ad01a4E805ed1b40723D6377b336eCcf"
    };

    // Initialize SDK and set the circles address
    async function initializeSdk() {
        const adapter = new BrowserProviderContractRunner();
        await adapter.init();
        setCirclesAddress(adapter.address);
        setSdk(new Sdk(chainConfig, adapter));
    }

    // Initialize SDK when component mounts
    useEffect(() => {
        initializeSdk();
    }, []); // Empty dependency array to run only once on mount

    // Fetch avatar information when sdk and circlesAddress are available
    useEffect(() => {
        async function fetchAvatar() {
            if (sdk && circlesAddress) {
                const avatar = await sdk.getAvatar(circlesAddress);
                setAvatarType(avatar.avatarInfo?.type || "Unknown");
                setAvatarAddress(circlesAddress);
            }
        }

        fetchAvatar();
    }, [sdk, circlesAddress]); // Runs when either sdk or circlesAddress updates

    return (
        <p>
            Avatar {avatarAddress} is {avatarType}
        </p>
    );
};

export default AvatarComponent;

```
{% endtab %}
{% endtabs %}
