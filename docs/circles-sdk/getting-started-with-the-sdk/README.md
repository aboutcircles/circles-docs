# 🚀 Getting started with the SDK

ou would require to mention your EOA private key, for this, you can also create `.env` file in your root folder where you can mention your Private Key and import directly in codebase.

To begin using the SDK, you require to setup these packages.

### Install the following packages :&#x20;

* `npm i ethers`
* `npm i circles-sdk`

### Step - 1 : Configure Circles SDK&#x20;

Once you have installed the above packages, we will configure the SDK.

{% code fullWidth="false" %}
```javascript
import { Sdk } from '@circles-sdk/sdk';
import {ethers} from "ethers";
```
{% endcode %}

### Step - 2 : Setup a signer

Our next step should be getting a signer. You can either use privatekey to derive wallet and use as signer param or simply use `window.ethereum` to use your metamask address.

{% tabs %}
{% tab title="window.ethereum" %}
<pre><code><strong>const provider = new ethers.BrowserProvider(window.ethereum);
</strong>const signer = await provider.getSigner();
console.log("Account:", await signer.getAddress()); //optionalsi
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

### Step - 3 : Get a RPC URL for Chiado&#x20;

Now, lets add RPC network. For time being, we will utilise Chiado testnet.

```javascript
const rpcURL = 'https://gnosis-chiado.drpc.org';
```

### Step - 4 : Setup Hub Contracts and migration contract

We will be setting V1 and V2 hub contracts which are deployed on Chiado testnet. This will allow us to use functions and events. We will dive in deeper to understand each contracts later. We will also add a migration contract which allows you to migrate your circles account between two different versions.

```javascript
const v1HubAddress = '0xdbf22d4e8962db3b2f1d9ff55be728a887e47710';
const v2HubAddress = '0xDA02CDB5279B3a1eF27Be3d91aE924495E6A5569';
const migration    = '0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0';
```

### Step - 5 : Initializing the Circles SDK

It's time to initialise the sdk using the hub contracts, migration contract, signer, and RPC url.

```javascript
const sdk = new Sdk(v1HubAddress, v2HubAddress, migration, signer, rpcURL);
```

Once you have succesfully initialized the SDK, you are all set to use Circles in your dApp. Let's learn more about circles SDK features and how you can use them.
