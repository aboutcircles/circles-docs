---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
---

# 🚀 Getting started with the SDK

### 1. Prerequisites

#### Install MetaMask and add Gnosis Chain & Chiado testnet

You'll need the MetaMask Plug-In installed in your browser: [https://metamask.io/](https://metamask.io/)

Once installed, you can check out the [official gnosis chain docs](https://docs.gnosischain.com/about/networks/) to get ChainID and RPC URL. Or you can quickly utilise the deep link for [Gnosis mainnet](https://docs.gnosischain.com/about/networks/mainnet) and [Chiado testnet](https://shanejonas.github.io/metamask-link/deep?method=wallet\_addEthereumChain\&params%5B0%5D%5BchainId%5D=0x27D8\&params%5B0%5D%5BchainName%5D=Chiado\&params%5B0%5D%5BrpcUrls%5D%5B0%5D=https://rpc.chiadochain.net\&params%5B0%5D%5BnativeCurrency%5D%5Bname%5D=Chiado%20xDAI\&params%5B0%5D%5BnativeCurrency%5D%5Bsymbol%5D=XDAI\&params%5B0%5D%5BnativeCurrency%5D%5Bdecimals%5D=18\&params%5B0%5D%5BblockExplorerUrls%5D%5B0%5D=https://blockscout.com/gnosis/chiado) .

{% hint style="info" %}
_You should use Chiado if you want to test a preview version of Circles v2. If you want to build with Circles v1, use Gnosis Chain instead._
{% endhint %}

#### Get some xDAI for paying gas fees

To send transactions you will need a small amount of xDai in order to pay for gas fees. You can get get xDAI tokens from mainnet and testnet faucet, depending on the need.

* Mainnet Faucet : [https://faucet.gnosischain.com/](https://faucet.gnosischain.com/)
* Testnet Faucet : [https://faucet.chiadochain.net/](https://faucet.chiadochain.net/)

### 2. Install packages

If you have all prerequisites in place, start by installing the Circles SDK package and ethers 6 in your project using npm.

* `npm i ethers`
* `npm i @circles-sdk/sdk@0.4.0 @circles-sdk/data@0.4.0`

### 2. Add imports

Then, import the necessary classes from the Circles SDK and ethers.

```typescript
import {Sdk, ChainConfig, SdkContractRunner} from "@circles-sdk/sdk";
import {BrowserProvider} from "ethers";
```

### 3. Chain specific configuration

Circles is available on Gnosis Chain and Chiado (the GC testnet). You need to specify the correct contract addresses and service endpoints for each environment.&#x20;

Choose from the following options, and copy & paste the selected `chainConfig` to your code.

{% tabs %}
{% tab title="Gnosis Chain " %}
The Gnosis Chain mainnet is the production chain for Circles. It currently only has the v1 contracts deployed.

```typescript
const chainConfig: ChainConfig = {
    pathfinderUrl: 'https://pathfinder.aboutcircles.com',
    circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
};
```
{% endtab %}

{% tab title="Chiado" %}
The Gnosis Chain testnet has both versions (v1 and v2) of the Circles contracts.

```typescript
const chainConfig: ChainConfig = {
    circlesRpcUrl: "https://chiado-rpc.aboutcircles.com",
    v1HubAddress: "0xdbf22d4e8962db3b2f1d9ff55be728a887e47710",
    v2HubAddress: "0x2066CDA98F98397185483aaB26A89445addD6740",
    migrationAddress: "0x2A545B54bb456A0189EbC53ed7090BfFc4a6Af94"
};
```
{% endtab %}
{% endtabs %}

### 4. Create a contract runner

A `SdkContractRunner` represents a way for the Sdk to sign messages and send them to the chain.

For the purpose of this guide, we'll show you two variants. One uses the `window.ethereum` object (as provided e.g. by MetaMask), the other uses a private key hex string.

{% tabs %}
{% tab title="MetaMask" %}
Here's how you can create one from the `window.ethereum` object that MetaMask provides.

```javascript
async function getRunner() : Promise<SdkContractRunner> {
    const w: any = window;
    const browserProvider = new BrowserProvider(w.ethereum);
    const signer = await browserProvider.getSigner();
    const address = await signer.getAddress();
    
    return {
        runner: signer,
        address: address
    };
}
```
{% endtab %}

{% tab title="Private key string" %}
If you want a private key string instead use the following code:

```typescript
async function getRunner() : Promise<SdkContractRunner> {
    const privateKey = 'your-private-key-hex-string';
    const provider = new JsonRpcProvider(chainConfig.circlesRpcUrl);
    const wallet = new Wallet(privateKey, provider);
    const address = await wallet.getAddress();

    return {
        runner: wallet,
        address: address
    };
}
```
{% endtab %}
{% endtabs %}

### 5. Initialize the Circles SDK

It's time to initialize the SDK using the ChainConfig and SdkContractRunner objects from above.

```typescript
async function getSdk() {
    const runner = await getRunner();
    return new Sdk(chainConfig, runner);
}
```

### 6. Use the SDK

You can now create an instance of the SDK by calling `getSdk()`. If you're using React you can e.g. store this instance in a Context to make it available in your application.

```javascript
const sdk = await getSdk();
```

Once you have successfully created a SDK instance, you are all set to use Circles in your dApp. Let's learn more about the Circles SDK features and how you can use them on the next pages.

### Full example

Here's the full code for Svelte and React for you to copy & paste:

{% tabs %}
{% tab title="Svelte" %}
```typescript
<script lang="ts">
    import {Sdk, type ChainConfig, type SdkContractRunner} from "@circles-sdk/sdk";
    import {BrowserProvider} from "ethers";
    import {onMount} from "svelte";

    let avatarAddress = "0xb235B56b91eccb9DbdF811D7b5C45c363AcaE98D";
    let avatarType = "";

    const chainConfig: ChainConfig = {
        pathfinderUrl: 'https://pathfinder.aboutcircles.com',
        circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
    };

    async function getRunner(): Promise<SdkContractRunner> {
        const w: any = window;
        const browserProvider = new BrowserProvider(w.ethereum);
        const signer = await browserProvider.getSigner();
        const address = await signer.getAddress();

        return {
            runner: signer,
            address: address
        };
    }

    async function getSdk() {
        const runner = await getRunner();
        return new Sdk(chainConfig, runner);
    }

    onMount(async () => {
        const sdk = await getSdk();
        const avatar = await sdk.getAvatar(avatarAddress);

        avatarType = avatar.avatarInfo?.type;
    });
</script>
<p>
    Avatar {avatarAddress} is {avatarType}
</p>
```
{% endtab %}

{% tab title="React" %}
```tsx
import React, { useEffect, useState } from "react";
import { Sdk, ChainConfig, SdkContractRunner } from "@circles-sdk/sdk";
import { BrowserProvider } from "ethers";

const AvatarComponent: React.FC = () => {
    const [avatarType, setAvatarType] = useState<string>("");
    const avatarAddress = "0xb235B56b91eccb9DbdF811D7b5C45c363AcaE98D";

    const chainConfig: ChainConfig = {
        pathfinderUrl: 'https://pathfinder.aboutcircles.com',
        circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
    };

    async function getRunner(): Promise<SdkContractRunner> {
        const w: any = window;
        const browserProvider = new BrowserProvider(w.ethereum);
        const signer = await browserProvider.getSigner();
        const address = await signer.getAddress();

        return {
            runner: signer,
            address: address
        };
    }

    async function getSdk() {
        const runner = await getRunner();
        return new Sdk(chainConfig, runner);
    }

    useEffect(() => {
        async function fetchAvatar() {
            const sdk = await getSdk();
            const avatar = await sdk.getAvatar(avatarAddress);
            setAvatarType(avatar.avatarInfo?.type || "");
        }

        fetchAvatar();
    }, []);

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
