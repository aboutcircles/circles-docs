---
icon: rectangles-mixed
---

# Quickstart Guide for Circles SDK

This guide explains how to use the latest `@aboutcircles` packages to build a browser-ready Circles dApp. Follow the steps to move from wallet prerequisites to a fully initialised `Sdk` instance capable of handling avatar reads, trust writes, transfers, profile updates, and the auxiliary helper surfaces exposed in v2.

### 1. Prerequisites

* Browser wallet such as MetaMask or Rabby.
* Gnosis Chain (chain ID `100`) configured inside the wallet. Double-check the RPC endpoint, currency symbol (`xDAI`), and block explorer entries via the [Gnosis Chain docs](https://docs.gnosischain.com/).
* A small amount of xDAI for gas (obtainable from the [mainnet faucet](https://gnosisfaucet.com/)).

Keep the wallet unlocked in the same browser context that will load your dApp so the runner can talk to `window.ethereum`.

### 2. Install the Circles SDK Packages

```bash
npm i @aboutcircles/sdk \
      @aboutcircles/sdk-core \
      @aboutcircles/sdk-types \
      @aboutcircles/sdk-runner \
      @aboutcircles/sdk-rpc \
      @aboutcircles/sdk-transfers \
      @aboutcircles/sdk-pathfinder \
      @aboutcircles/sdk-profiles \
      viem
```

Install whichever modules you plan to import directly. `@aboutcircles/sdk` already depends on the others, but pinning them in your app keeps the versions aligned across bundlers.

* `@aboutcircles/sdk` – high-level surface your UI consumes (avatars, registration, helper namespaces).
* `@aboutcircles/sdk-core` – typed contract wrappers plus `circlesConfig`.
* `@aboutcircles/sdk-types` – shared types including `ContractRunner`, `TransactionRequest`, and avatar data models.
* `@aboutcircles/sdk-runner` – production runners (`SafeContractRunner`, `SafeBrowserRunner`) and error helpers.
* `@aboutcircles/sdk-rpc` – typed JSON-RPC client for balances, trust graphs, pathfinding, transactions, and events.
* `@aboutcircles/sdk-transfers` – `TransferBuilder` orchestration around pathfinding + wrapper management; it leans on…
* `@aboutcircles/sdk-pathfinder` – utilities for flow matrices, wrapper lookups, and path manipulation.
* `@aboutcircles/sdk-profiles` – profile pinning client that the SDK uses internally for metadata updates.
* `viem` – RPC + wallet tooling shared by the runner implementations.

### 3. Import the SDK Building Blocks

```typescript
import { Sdk } from '@aboutcircles/sdk';
import { Core, circlesConfig, type CirclesConfig } from '@aboutcircles/sdk-core';
import type { ContractRunner } from '@aboutcircles/sdk-types';
import { createPublicClient, createWalletClient, custom, http } from 'viem';
import { gnosis } from 'viem/chains';

// Optional modules for lower-level access when you need them
import { CirclesRpc } from '@aboutcircles/sdk-rpc';
import { TransferBuilder } from '@aboutcircles/sdk-transfers';
import { Profiles } from '@aboutcircles/sdk-profiles';
```

* `Sdk` is the primary entry point exposing avatar namespaces (`balances`, `trust`, `transfer`, `profile`, etc.).
* `Core`/`circlesConfig` provide production-ready contract addresses, service URLs, and default RPC endpoints.
* `ContractRunner` defines the interface your signing strategy (EOA, Safe, hardware wallet, …) must satisfy.
* `CirclesRpc`, `TransferBuilder`, and `Profiles` are optional direct imports when you need to bypass the high-level helpers (analytics views, offline tooling, custom flows).
* `viem` primitives make the runner portable across browser wallets and Node environments.

Need multisig support? `@aboutcircles/sdk-runner` exports `SafeContractRunner` (backend) and `SafeBrowserRunner` (front-end Safe Apps). Swap your custom runner with those when you integrate Safe-based flows.

### 4. Choose the Right Network Configuration

#### Production (Gnosis Chain Mainnet)

```typescript
const gnosisMainnetConfig = circlesConfig[100];
```

This object already contains:

* Service URLs: `circlesRpcUrl` (aggregated Circles RPC), `pathfinderUrl`, and `profileServiceUrl`.
* Canonical contract addresses: `v1HubAddress`, `v2HubAddress`, `nameRegistryAddress`, `invitationEscrowAddress`, `baseGroupFactoryAddress`, `baseGroupMintPolicy`, `standardTreasury`, `coreMembersGroupDeployer`, and `liftERC20Address`.
* Metadata used internally by `TransferBuilder`, `CirclesRpc`, and the avatar helpers (so you rarely need to hard-code addresses).



### 5. Build a Browser-Friendly Contract Runner

An EOA runner is ideal for MetaMask/Rabby-style single-signers. The snippet below shows a minimal implementation that satisfies the `ContractRunner` interface with the help of `viem`:

```typescript
import { createPublicClient, createWalletClient, custom, http } from 'viem';
import { gnosis } from 'viem/chains';
import type { Address, TransactionReceipt, TransactionRequest } from '@aboutcircles/sdk-types';

const circlesRPC = 'https://rpc.aboutcircles.com';

export const createBrowserRunner = () => {
  const publicClient = createPublicClient({
    chain: gnosis,
    transport: http(circlesRPC),
  });

  const walletClient = createWalletClient({
    chain: gnosis,
    transport: custom(window.ethereum),
  });

  const runner: ContractRunner = {
    publicClient,
    address: walletClient.account?.address,
    async init() {
      const [account] = await window.ethereum.request({ method: 'eth_requestAccounts' });
      this.address = account as Address;

      const currentChain = await walletClient.getChainId();
      if (currentChain !== gnosis.id) {
        throw new Error('Please switch your wallet to Gnosis Chain (chainId 100).');
      }
    },
    estimateGas: (tx: TransactionRequest) =>
      publicClient.estimateGas({ account: runner.address!, ...tx }),
    call: (tx: TransactionRequest) =>
      publicClient.call({ account: tx.from || runner.address!, ...tx }),
    resolveName: (name: string) => publicClient.getEnsAddress({ name }),
    async sendTransaction(txs: TransactionRequest[]): Promise<TransactionReceipt> {
      if (!runner.address) throw new Error('Runner not initialised. Call init() first.');

      let receipt: TransactionReceipt | undefined;
      for (const tx of txs) {
        const hash = await walletClient.sendTransaction({
          account: runner.address,
          ...tx,
        });
        receipt = await publicClient.waitForTransactionReceipt({ hash });
      }

      if (!receipt) {
        throw new Error('No transactions submitted.');
      }
      return receipt;
    },
  };

  return runner;
};
```

Key behaviors:

* `init()` requests wallet access and enforces the correct chain ID before any on-chain action.
* `sendTransaction()` loops through every `TransactionRequest` emitted by avatar helpers or `TransferBuilder`, ensuring multi-step flows (pathfinding, wrapped transfers, etc.) complete in order.
* Gas estimation, read-only calls, and ENS resolution are delegated to `viem` so you do not need to reinvent RPC plumbing.

Prefer a Safe-based UX? Replace the custom runner with `SafeBrowserRunner.create(rpcUrl, window.ethereum, safeAddress, gnosis)` (or the server-side `SafeContractRunner`) from `@aboutcircles/sdk-runner`. Both satisfy the same interface.

### 6. Tap Into Lower-Level Packages

When you need extra control—analytics dashboards, dry-run tooling, custom transfer flows—instantiate the underlying modules alongside the `Sdk`:

```typescript
import { Core, circlesConfig } from '@aboutcircles/sdk-core';
import { CirclesRpc } from '@aboutcircles/sdk-rpc';
import { TransferBuilder } from '@aboutcircles/sdk-transfers';
import { Profiles } from '@aboutcircles/sdk-profiles';

const config = circlesConfig[100];
const core = new Core(config);
const rpc = new CirclesRpc(config.circlesRpcUrl);
const transferBuilder = new TransferBuilder(core);
const profilesClient = new Profiles(config.profileServiceUrl);
```

* `Core` returns raw transaction requests for HubV2, BaseGroups, Lift ERC-20 wrappers, etc.
* `CirclesRpc` streams balances, trust graphs, profile search results, and transaction history straight from the Circles indexers.
* `TransferBuilder` leverages `@aboutcircles/sdk-pathfinder` to create the ordered transaction list (unwraps, approvals, `operateFlowMatrix`) without sending anything.
* `Profiles` is handy for batch jobs or when you want to pin metadata outside of the avatar helpers.

Reuse the same `config` everywhere to avoid mismatched service URLs.

### 7. Initialise the Circles SDK

```typescript
import { Sdk } from '@aboutcircles/sdk';
import { circlesConfig } from '@aboutcircles/sdk-core';

const runner = createBrowserRunner();
await runner.init();

const sdk = new Sdk(circlesConfig[100], runner); // config defaults to mainnet if omitted

const avatar = await sdk.getAvatar(runner.address!);
const balances = await avatar.balances.getTokenBalances();
const trustGraph = await sdk.data.getTrustRelations(avatar.address);
const demurragedWrapper = await sdk.tokens.getDemurragedWrapper(avatar.address);

const members = sdk.groups.getMembers('0xGroupAddress...');
await members.queryNextPage(); // fetch first page when you need it
```

* Passing `circlesConfig[100]` explicitly removes any ambiguity about the targeted environment.
* Provide your custom (or Safe) runner so avatar helpers can sign and broadcast state-changing transactions. If you leave the runner undefined, the SDK functions remain read-only.
* `sdk.data` exposes RPC-backed helpers for avatars, trust, and balances when you do not need a full avatar instance.
* `sdk.tokens` contains convenience helpers for wrappers and token-holder pagination.
* `sdk.groups` helps explore group treasuries, membership lists, and holder breakdowns without writing direct RPC calls.
* `sdk.profiles` offers direct `create/get` helpers powered by `@aboutcircles/sdk-profiles`.

### 8. End-to-End Setup Example

```typescript
import { Sdk } from '@aboutcircles/sdk';
import { circlesConfig } from '@aboutcircles/sdk-core';
import { createBrowserRunner } from './runner'; // use the helper from section 5

async function bootstrapCircles() {
  const runner = createBrowserRunner();
  await runner.init();

  const sdk = new Sdk(circlesConfig[100], runner);
  const avatar = await sdk.getAvatar(runner.address!);

  console.log(await avatar.profile.get());

  const trustGraph = await sdk.data.getTrustRelations(avatar.address);
  console.log(`Trustees: ${trustGraph.length}`);

  const topHolders = sdk.tokens.getHolders(avatar.address, 3);
  await topHolders.queryNextPage();
  console.log('Top holders', topHolders.currentPage?.results ?? []);
}

bootstrapCircles().catch(console.error);
```

This pattern maps cleanly to a React `useEffect`, a Next.js server action (with a server-side runner), or a plain script.

### 9. Choosing SDK Surfaces

Pick the surfaces that match your UX:

* **HumanAvatar / OrganisationAvatar / BaseGroupAvatar** – mint issuance, manage trust, transfer CRC, wrap/unwrap, and maintain profiles with type-specific helpers.
* **`sdk.register`** – onboard new humans, organisations, and base groups while handling invitation escrows and profile pinning.
* **`sdk.data`** – lightweight RPC views for trust graphs, balances, and avatar metadata (great for dashboards).
* **`sdk.tokens`** – wrapper lookups plus cursor-based holder queries for ERC1155/ERC20 representations.
* **`sdk.groups`** – fetch group members, treasury balances, and holder distributions without juggling contracts manually.
* **`sdk.profiles`** – talk to the profile service directly when you need to pin/update metadata outside avatar flows.

Decide which combos your application needs up front so you can import only the modules you plan to expose.

