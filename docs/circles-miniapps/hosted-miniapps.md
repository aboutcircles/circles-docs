# Hosted MiniApps

This is a practical, end-to-end technical guide for building miniapps that run inside the Circles miniapp host and communicate through `@aboutcircles/miniapp-sdk`.

The guide is intentionally generic: it does not assume any specific app domain (groups, treasury, profiles, etc.). Use it as a production blueprint for any miniapp feature set.

### 1. Mental Model: How Miniapps Run

A Circles-compatible miniapp is a web app rendered in an iframe inside a host application.

* Your app owns UI, local state, validation, and business flow.
* The host owns wallet session UX and transaction approval.
* `@aboutcircles/miniapp-sdk` bridges both sides using `postMessage`.

This means:

* Local browser testing is useful for UI and logic.
* End-to-end wallet/transaction testing must happen in the host.

### 2. SDK Capabilities (Current Surface)

The SDK currently exposes:

* `isMiniappMode(): boolean`
* `onAppData(fn: (data: string) => void): void`
* `onWalletChange(fn: (address: string | null) => void): () => void`
* `sendTransactions(transactions: Transaction[]): Promise<string[]>`
* `signMessage(message: string, signatureType?: 'erc1271' | 'raw'): Promise<{ signature: string; verified: boolean }>`

Where `Transaction` is:

```ts
type Transaction = {
  to: string;
  data?: string;
  value?: string; // hex string, e.g. "0x0"
};
```

### 3. Project Setup

#### 3.1 Create the app

```bash
npm create vite@latest my-miniapp -- --template vanilla
cd my-miniapp
npm i
npm i @aboutcircles/miniapp-sdk viem
```

Add additional Circles SDK packages only if your use case needs them:

```bash
npm i @aboutcircles/sdk-core @aboutcircles/sdk-rpc @aboutcircles/sdk-profiles @aboutcircles/sdk-transfers @aboutcircles/sdk-utils
```

#### 3.2 Suggested structure

```
src/
  app/
    state.js
    actions.js
    ui.js
  host/
    bridge.js
  chain/
    client.js
  utils/
    format.js
    validation.js
main.js
```

Keep host bridge logic isolated from domain logic.

### 4. Bootstrapping the Host Bridge

Create a dedicated bridge module:

```js
// src/host/bridge.js
import {
  isMiniappMode,
  onAppData,
  onWalletChange,
  sendTransactions,
  signMessage,
} from '@aboutcircles/miniapp-sdk';

export { isMiniappMode, onAppData, onWalletChange, sendTransactions, signMessage };
```

Use `onWalletChange` as the app’s wallet truth source.

### 5. Wallet Lifecycle Pattern

Implement wallet lifecycle as a finite-state flow:

1. `disconnected`
2. `connecting` (optional UI state)
3. `connected`
4. `error` (recoverable or fatal)

Minimal pattern:

```js
import { getAddress } from 'viem';
import { onWalletChange } from './host/bridge.js';

let connectedAddress = null;

onWalletChange(async (address) => {
  try {
    connectedAddress = address ? getAddress(address) : null;
  } catch {
    connectedAddress = null;
  }

  if (!connectedAddress) {
    resetAccountScopedState();
    renderDisconnected();
    return;
  }

  resetAccountScopedState();
  await loadInitialDataFor(connectedAddress);
  renderConnected(connectedAddress);
});
```

Rules:

* Always reset account-scoped caches on wallet changes.
* Never assume prior in-memory state is still valid after reconnects.

### 6. Transaction Submission Pattern

Always format and submit transactions through one adapter.

```js
import { sendTransactions } from './host/bridge.js';

function toHexValue(value) {
  return value ? `0x${BigInt(value).toString(16)}` : '0x0';
}

export function formatTxForHost(tx) {
  return {
    to: tx.to,
    data: tx.data || '0x',
    value: toHexValue(tx.value || 0n),
  };
}

export async function submitTransactions(txs) {
  return sendTransactions(txs.map(formatTxForHost));
}
```

Why this is critical:

* Prevents value-encoding inconsistencies.
* Keeps write paths auditable.
* Makes retries and logging straightforward.

### 7. Standard Action Pipeline

For every write action (any state-changing on-chain operation), use this pipeline:

1. Validate input
2. Normalize addresses/amounts
3. Optional preflight simulation
4. Build one or more tx payloads
5. Show pending UI
6. Submit via host bridge
7. Wait for confirmations (if required by UX)
8. Show success/failure
9. Refresh affected data

Example shell:

```js
export async function performAction(input) {
  validateInput(input);

  const txs = buildTransactions(input);
  setStatus('pending', 'Submitting transaction...');

  try {
    const hashes = await submitTransactions(txs);
    await maybeWaitForReceipts(hashes);
    setStatus('success', `Submitted ${hashes.length} transaction(s).`);
    await reloadRelevantState();
  } catch (err) {
    setStatus('error', normalizeError(err));
  }
}
```

### 8. Signing Flows (`signMessage`)

Use `signMessage` when your backend or protocol needs host-backed signatures.

```js
import { signMessage } from './host/bridge.js';

const result = await signMessage('Example message', 'erc1271');
// result: { signature, verified }
```

Guidance:

* Prefer `'erc1271'` unless you explicitly require raw bytes semantics.
* Persist signature type with the signature payload for verifier correctness.

### 9. App Data Injection (`onAppData`)

The host can pass app-specific context via query data.

Use cases:

* entry context (resource id, mode, source)
* feature flags
* campaign or referral metadata

Pattern:

```js
import { onAppData } from './host/bridge.js';

onAppData((raw) => {
  const data = safelyParseHostData(raw); // your parser/validator
  applyHostContext(data);
});
```

Never trust raw host data without validation.

### 10. Validation and Type Safety

Minimum validation set:

* `address`: checksum normalization, strict format checks
* `amount`: decimal precision + non-negative + max bounds
* `url`: protocol allowlist (`https://` by default)
* `text`: length limits and rendering-safe escaping

Keep pure validation helpers in a dedicated module and test them directly.

### 11. Error Handling and Recovery

Design a predictable error model:

* `validation_error`
* `user_rejected`
* `network_error`
* `host_bridge_error`
* `unexpected_error`

Normalize unknown errors:

```js
export function normalizeError(err) {
  if (!err) return 'Unknown error';
  if (typeof err === 'string') return err;
  return err.shortMessage || err.message || String(err);
}
```

For recoverable host issues:

* show actionable guidance
* preserve user input where safe
* do not silently retry state-changing actions

### 12. Concurrency and Race Control

Use request IDs for async operations that may race:

```js
let reqId = 0;

export async function runLatest(task) {
  const id = ++reqId;
  const result = await task();
  if (id !== reqId) return null; // stale
  return result;
}
```

Apply to:

* debounced search
* paginated loads
* wallet-dependent initialization

### 13. Performance Practices

* Debounce user-driven remote search.
* Cache immutable reference data.
* Batch on-chain reads where available.
* Defer non-critical UI hydration.
* Split vendor chunks in build output.

Avoid heavy blocking work on input/change handlers.

### 14. Security Checklist

* Never hardcode private keys, tokens, or secrets in frontend.
* Escape/sanitize all user-controlled HTML output.
* Validate all host-injected and URL-provided data.
* Re-validate critical preconditions before tx submission.
* Protect against accidental double-submission (disable buttons during pending).
* Keep domain allowlists for outbound links.

### Generic Starter Skeleton

```js
import { getAddress, isAddress } from 'viem';
import {
  isMiniappMode,
  onWalletChange,
  sendTransactions,
} from '@aboutcircles/miniapp-sdk';

let connected = null;

function toHexValue(value) {
  return value ? `0x${BigInt(value).toString(16)}` : '0x0';
}

function status(msg) {
  document.getElementById('status').textContent = msg;
}

async function sendExample() {
  if (!connected || !isAddress(connected)) throw new Error('Wallet not connected');
  const txs = [{ to: connected, data: '0x', value: toHexValue(0n) }];
  const hashes = await sendTransactions(txs);
  status(`Submitted: ${hashes.join(', ')}`);
}

onWalletChange((address) => {
  try {
    connected = address ? getAddress(address) : null;
  } catch {
    connected = null;
  }
  status(connected ? `Connected: ${connected}` : 'Disconnected');
});

status(isMiniappMode() ? 'Host mode' : 'Standalone mode');
document.getElementById('send').addEventListener('click', () => {
  sendExample().catch((err) => status(`Error: ${err.message}`));
});
```
