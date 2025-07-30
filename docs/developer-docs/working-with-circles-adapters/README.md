---
icon: plug
---

# Working with Circles Adapters

The Circles SDK ([@circles-sdk/sdk](https://www.npmjs.com/package/@circles-sdk)) requires a pluggable transaction execution layer abstracted by the SdkContractRunner interface defined in @circles-sdk/adapter. This abstraction enables the SDK to interact with the Circles protocol smart contracts deployed on Gnosis Chain without assuming a specific signing mechanism.

Core interface types and abstractions for building pluggable contract runners for Circles SDK.

This package defines the core types used to interact with smart contracts, whether on-chain via wallets or programmable signers.

***

### &#x20;Installation

```bash
npm install @circles-sdk/adapter
```

***

### &#x20;Interfaces

#### `TransactionRequest`

Represents a generic transaction to be sent.

```ts
type TransactionRequest = {
  readonly to: Address;
  readonly value: bigint;
  readonly data: string;
};
```

***

#### `TransactionResponse`

Represents a response from a successfully sent transaction.

```ts
type TransactionResponse = {
  blockNumber: number;
  blockHash: string;
  index: number;
  hash: string;
  type: number;
  to: Address;
  from: Address;
  gasLimit: bigint;
  gasPrice: bigint;
  data: string;
  value: bigint;
  chainId: number;
};
```

***

#### `SdkContractRunner`

An abstraction for submitting and simulating transactions. Use this when integrating with external signing environments (Safe, WalletConnect, PrivateKey, etc).

```ts
interface SdkContractRunner {
  estimateGas?: (tx: TransactionRequest) => Promise<bigint>;
  call?: (tx: TransactionRequest) => Promise<string>;
  resolveName?: (name: string) => Promise<null | string>;
  sendTransaction?: (tx: TransactionRequest) => Promise<TransactionResponse>;
  address?: Address;
  sendBatchTransaction?: () => BatchRun;
}
```

**Common Use Cases**

* **Gas Estimation**: Estimate cost before sending.
* **Static Call**: Use `.call()` for `view` or `pure` functions.
* **ENS Resolution**: Resolve human-readable names to addresses.
* **Send Transaction**: Mutate state via `.sendTransaction()`.
* **Batch Transaction**: Execute multiple txs atomically via `sendBatchTransaction()`.

***

#### `BatchRun`

Encapsulates logic for bundling and executing a batch of transactions (e.g., via a Safe multisend).

```ts
interface BatchRun {
  addTransaction: (tx: TransactionRequest) => void;
  getTxCalldata?: () => Promise<EthSafeTransaction>;
  run: () => Promise<TransactionResponse>;
}
```

***

This package is typically used with one of the adapters such as:

* [`@circles-sdk/adapter-safe`](safe-adapter.md)
* [`@circles-sdk/adapter-ethers`](ethers-adapter.md)

