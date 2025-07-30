# Safe Adapter

A contract runner adapter that enables interaction with [Safe](https://safe.global) smart contracts using the Circles SDK. It supports both browser-based wallet integrations and private key execution environments.

***

### &#x20;Features

* Execute Safe transactions via private key or browser wallet.
* Support for batch transactions using Safe's multisig features.
* Compatible with the Circles SDK's `SdkContractRunner` interface.
* Seamless integration with Safe Protocol Kit v1.

***

### &#x20;Installation

```bash
npm install @circles-sdk/adapter-safe
```

***

### &#x20;Usage

#### 1. `SafeSdkPrivateKeyContractRunner`

Used when you want to sign and execute Safe transactions using a private key and RPC URL (e.g., for server-side usage).

```ts
import { SafeSdkPrivateKeyContractRunner } from '@circles-sdk/adapter-safe';

const runner = new SafeSdkPrivateKeyContractRunner(
  '0xYOUR_PRIVATE_KEY',
  'https://rpc.gnosischain.com'
);

await runner.init('0xYourSafeAddress');

// Send a transaction
await runner.sendTransaction?.({
  to: '0xRecipientAddress',
  value: BigInt(0),
  data: '0x',
});
```

***

#### 2. `SafeSdkBrowserContractRunner`

Used for browser environments, using injected EIP-1193 providers (e.g., MetaMask).

```ts
import { SafeSdkBrowserContractRunner } from '@circles-sdk/adapter-safe';

const runner = new SafeSdkBrowserContractRunner();
```
