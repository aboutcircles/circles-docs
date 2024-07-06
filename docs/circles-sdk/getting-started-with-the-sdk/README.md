---
description: >-
  This guide will help you get started with the Circles SDK. It shows how to use
  the Circles SDK with MetaMask.
---

# 🚀 Getting started with the SDK

### 1. Prerequisites

#### Install MetaMask and add Gnosis Chain & Chiado

You'll need the MetaMask Plug-In installed in your browser:

* [https://metamask.io/](https://metamask.io/)

Once installed, please make sure to add all the chains you want to use.&#x20;

* [https://docs.gnosischain.com/tools/wallets/metamask/](https://docs.gnosischain.com/tools/wallets/metamask/)

_You should use Chiado if you want to test a preview version of Circles v2. If you want to build with Circles v1 use Gnosis Chain instead._

#### Get some xDai for gas fees

To send transactions you will need a small amount of xDai in order to pay for gas fees. You can get some from a faucet:

* [https://docs.gnosischain.com/tools/Faucets](https://docs.gnosischain.com/tools/Faucets)

### 2. Install packages

If you have all prerequisites in place, start by installing the Circles SDK package and ethers 6 in your project using npm.

* `npm i ethers`
* `npm i @circles-sdk/sdk@0.3.0`

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
TBD
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
