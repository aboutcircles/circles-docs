# 🚀 Getting started with the SDK

To begin using the SDK, you require to setup these packages.

### Install the following packages :&#x20;

* `npm i ethers`
* `npm i @circles-sdk/sdk@0.0.44`

### Step - 1 : Configure Circles SDK&#x20;

Once you have installed the above packages, we will configure the SDK. We will also import `ChainConfig` to be able to utilise circles RPC in next steps.

{% code fullWidth="false" %}
```javascript
import { Sdk } from '@circles-sdk/sdk';
import {ethers} from "ethers";
import { ChainConfig } from "@circles-sdk/sdk";
```
{% endcode %}

### Step - 2 : Setup RPC and signer

We will be using the custom RPC for circles

```
const chainConfig: ChainConfig = {
  circlesRpcUrl: 'https://rpc.aboutcircles.com',
};
```

Next should be getting a signer. You can either use privatekey to derive wallet and use as signer param or simply use `window.ethereum` to use your metamask address.

{% tabs %}
{% tab title="window.ethereum" %}
<pre><code><strong>const provider = new ethers.BrowserProvider(window.ethereum);
</strong>const signer = await provider.getSigner();
console.log("Account:", await signer.getAddress());
</code></pre>
{% endtab %}

{% tab title="Using private key" %}
You would require to mention your EOA private key, for this, you can also create `.env` file in your root folder where you can mention your Private Key and import directly in codebase.

{% code title=".env" overflow="wrap" %}
```javascript
PRIVATE_KEY = '0x..';
```
{% endcode %}

Let's add the private key to main application file and get the wallet.

```javascript
const privatekey = process.env.PRIVATE_KEY
const jsonRpcProvider = new ethers.JsonRpcProvider(rpcUrl);
const wallet = new ethers.Wallet(privateKey, jsonRpcProvider);            
```

You can setup the rpcUrl from the next step.
{% endtab %}
{% endtabs %}

### Step - 3 : Initializing the Circles SDK

It's time to initialise the sdk using the signer and RPC url.

```javascript
const sdk = new Sdk(chainConfig, signer);
```

Once you have successfully initialized the SDK, you are all set to use Circles in your dApp. Let's learn more about circles SDK features and how you can use them.
