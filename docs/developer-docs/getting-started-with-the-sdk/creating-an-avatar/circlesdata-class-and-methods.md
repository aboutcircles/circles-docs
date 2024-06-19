---
description: You can utilise to get info related to avatar using @circles-sdk/data class.
---

# 📝 CirclesData class and methods

Let's start by setting up classes from @circles-sdk/data package.

```javascript
import { CirclesData, CirclesRpc } from '@circles-sdk/data';
```

```javascript
const RPC = new circlesRpcUrl(chainConfig.circlesRpcUrl);
const data = new CirclesData(RPC);
```

Here are the list of methods you can call from class if you already have initialized the SDK by setting up.

```javascript
const sdk = new Sdk(chainConfig, signer);
```

1. **To get Avatar Info**:&#x20;

This will include the signup timestamp, circles version, avatar type and token address/ID.

```javascript
sdk.data.getAvatarInfo("0x1234");
```

2. **To get total balance of an Avatar:**&#x20;

```javascript
sdk.data.getTotalBalance("0x1234");
```

3. **To get token balances of an Avatar:**

```javascript
sdk.data.getTokenBalances("0x1234");
```

4. **To get transaction history of an Avatar:**

<pre class="language-javascript"><code class="lang-javascript"><strong>sdk.data.getTransactionHistory("0x1234");
</strong></code></pre>

5. To get the current incoming and outgoing trust relations of an address

```javascript
sdk.data.getTrustRelations("0x1234");
```
