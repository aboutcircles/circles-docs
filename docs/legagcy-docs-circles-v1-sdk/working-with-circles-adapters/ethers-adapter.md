# Ethers Adapter

This package provides Ethers-compatible contract runners for the Circles SDK. It enables interaction with smart contracts using Ethers v6 providers, wallets, and browser environments like MetaMask.

***

### &#x20;Installation

```bash
npm install @circles-sdk/adapter-ethers
```

***

### &#x20;Overview

This package implements the `SdkContractRunner` interface from `@circles-sdk/adapter`, making it usable with any part of the Circles SDK that expects a contract runner.

***

### Supported Runners

#### 1. `PrivateKeyContractRunner`

For backend/server usage with a private key and a JSON-RPC provider.

```ts
import { PrivateKeyContractRunner } from '@circles-sdk/adapter-ethers';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc.gnosischain.com');
const runner = new PrivateKeyContractRunner(provider, '0xYOUR_PRIVATE_KEY');

await runner.init();
await runner.sendTransaction?.({
  to: '0xRecipientAddress',
  value: BigInt(0),
  data: '0x',
});
```

***

#### 2. `BrowserProviderContractRunner`

For browser-based wallets such as MetaMask using `window.ethereum`.

```ts
import { BrowserProviderContractRunner } from '@circles-sdk/adapter-ethers';

const runner = new BrowserProviderContractRunner();
await runner.init();

await runner.sendTransaction?.({
  to: '0xRecipientAddress',
  value: BigInt(0),
  data: '0x',
});
```

***

#### 3. `SdkContractRunnerWrapper`

Wraps any `SdkContractRunner` to be used with Ethers contract factory or TypeChain-generated wrappers.

```ts
import { SdkContractRunnerWrapper } from '@circles-sdk/adapter-ethers';

const wrapper = new SdkContractRunnerWrapper(provider, address, sdkRunner);
await wrapper.sendTransaction?.({
  to: '0xRecipient',
  value: BigInt(0),
  data: '0x...',
});
```

***

### &#x20;Batch Execution

#### `PrivateKeyBatchRun` and `BrowserProviderBatchRun`

All runners support batching multiple transactions and executing them in sequence.

```ts
const batch = runner.sendBatchTransaction?.();
batch?.addTransaction({
  to: '0x...',
  value: BigInt(0),
  data: '0x...',
});
await batch?.run();
```
