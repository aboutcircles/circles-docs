# 🚀 Getting started with the SDK

### Install the following packages :&#x20;

* `npm i ethers`
* `npm i circles-sdk`

### Configure Circles SDK&#x20;

Once you have installed the above packages, we will configure the SDK.

{% code fullWidth="false" %}
```javascript
import { Sdk } from '@circles-sdk/sdk';
import {ethers} from "ethers";
```
{% endcode %}

ou would require to mention your EOA private key, for this, you can also create `.env` file in your root folder where you can mention your Private Key and import directly in codebase.

{% code title=".env" %}
```javascript
PRIVATE_KEY = '0x..';
```
{% endcode %}

<pre class="language-javascript"><code class="lang-javascript"><strong>import { Sdk } from '@circles-sdk/sdk';
</strong>import {ethers} from "ethers";

const privatekey = process.env.PRIVATE_KEY                        
</code></pre>

{% tabs %}
{% tab title="Gnosis Chain" %}

{% endtab %}

{% tab title="Chiado Testnet" %}

{% endtab %}

{% tab title="Anvil - Localhost" %}

{% endtab %}
{% endtabs %}

Now, lets add RPC network. If you would like to run this application locally, you can setup RPC using localhost port or use Gnosis Chain/Chiado's RPC url. For reference, let's use localhost port 8545.

```javascript
const rpcUrl = 'http://localhost:8545';
```

Set up V1 and V2 Hub Contracts :

```javascript
const v1HubAddress = '0x5FbDB2315678afecb367f032d93F642f64180aa3';
const v2HubAddress = '0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512';
```

Now let's connect to RPC using json RPC provider or you can use any other RPC provider. We will then initialize the SDK.



Signers :

{% tabs %}
{% tab title="ethers.js" %}

{% endtab %}

{% tab title="web3.js" %}

{% endtab %}

{% tab title="wagmi.js" %}

{% endtab %}

{% tab title="walletconnect" %}

{% endtab %}
{% endtabs %}

```javascript
const jsonRpcProvider = new ethers.JsonRpcProvider(rpcUrl);
const wallet = new ethers.Wallet(privateKey, jsonRpcProvider);
const sdk = new Sdk(v1HubAddress, v2HubAddress, wallet);
```

Once you have succesfully initialized the SDK, you are all set to use Circles in your dApp. Let's learn more about circles SDK features and you can use them.
