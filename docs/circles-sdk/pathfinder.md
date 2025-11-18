---
icon: bezier-curve
---

# Pathfinder

Circles pathfinder calculates “how much and through which tokens can flow from A to B” based on trust and balances (wrapped and unwrapped). You can use it to:

* Preview a route and achievable amount.
* Feed `TransferBuilder` or custom `operateFlowMatrix` calls.

### Entry points

* High-level SDK: `const rpc = (sdk as any).rpc; rpc.pathfinder.findPath / findMaxFlow`
* Standalone: `const rpc = new CirclesRpc(config.circlesRpcUrl);`

#### Methods

* `findPath(params: FindPathParams): Promise<PathfindingResult>`
* `findMaxFlow(params: Omit<FindPathParams, 'targetFlow'>): Promise<bigint>`

### Parameters (FindPathParams)

* `from` (Address, required): Source avatar.
* `to` (Address, required): Destination avatar.
* `targetFlow` (bigint, required for `findPath`): Desired amount.
* `useWrappedBalances` (boolean, optional): Allow ERC20 wrappers in the route. **Set true if you hold wrapped CRC.**
* `fromTokens` / `toTokens` (Address\[], optional): Only allow these token owners from sender/recipient sides.
* `excludeFromTokens` / `excludeToTokens` (Address\[], optional): Ban specific token owners.
* `simulatedBalances` (array, optional): Inject hypothetical balances `{ holder, token, amount, isWrapped, isStatic }`.
* `maxTransfers` (number, optional): Hop cap; lower = faster, but may reduce flow.

### `findPath` (targeted route)

Computes a path for a requested amount; returns what is actually achievable plus hop-by-hop legs.

```ts
import { CirclesRpc } from '@aboutcircles/sdk-rpc';

const rpc = new CirclesRpc('https://rpc.circlesubi.network/');

const path = await rpc.pathfinder.findPath({
  from: '0xSender',
  to: '0xRecipient',
  targetFlow: BigInt('10000000000000000000'), // 10 CRC
  useWrappedBalances: true,
  fromTokens: ['0xPreferredToken'],  // optional pin
  excludeToTokens: ['0xAvoidToken'], // optional ban
  maxTransfers: 4
});

console.log(path.maxFlow);    // bigint achievable
console.log(path.transfers);  // [{ from, to, tokenOwner, value }]
```

`PathfindingResult`:

* `maxFlow` – achievable flow for this request.
* `transfers` – ordered legs; `tokenOwner` is the ERC1155 owner or wrapper used in that hop.

If `maxFlow < targetFlow`, decide to scale down or show an error.

### `findMaxFlow` (ceiling)

Returns the absolute maximum possible flow between two avatars with the same options.

```ts
const max = await rpc.pathfinder.findMaxFlow({
  from: '0xSender',
  to: '0xRecipient',
  useWrappedBalances: true,
  simulatedBalances: [
    { holder: '0xSender', token: '0xToken', amount: 5n * 10n ** 18n, isWrapped: false, isStatic: false }
  ],
});
```

Use for sliders, validation, or deciding if you must unwrap/wrap first.

### Advanced usage

* **Wrapped tokens:** `useWrappedBalances: true` enables consuming inflationary/demurraged ERC20 wrappers the sender holds.
* **Token pinning/bans:** Use `fromTokens`/`toTokens` to force specific personal or group tokens; use the `exclude*` variants to forbid costly or undesired wrappers.
* **Simulations:** `simulatedBalances` let you preview flows after an expected mint/wrap without waiting for indexer updates.
* **Hop limit:** `maxTransfers` curbs route length; helpful for UX responsiveness but may lower `maxFlow`.

### From path to execution

* Easiest: call `TransferBuilder.constructAdvancedTransfer(from, to, amount, options)`—it runs `findPath`, adds unwrap/rewrap, approvals, and returns an ordered tx array.
* Manual: convert `PathfindingResult` to `operateFlowMatrix`:

```ts
import { createFlowMatrix } from '@aboutcircles/sdk-pathfinder';
import { Core } from '@aboutcircles/sdk-core';

const core = new Core();
const rpc = new CirclesRpc(core.config.circlesRpcUrl);

const path = await rpc.pathfinder.findPath({ from, to, targetFlow, useWrappedBalances: true });
const flow = createFlowMatrix(from, to, path.maxFlow, path.transfers);

const tx = core.hubV2.operateFlowMatrix(
  flow.flowVertices,
  flow.flowEdges,
  flow.streams,
  flow.packedCoordinates
);
// send tx via your runner
```

### Pathfinder helper functions (when you build flows yourself)

* `createFlowMatrix(from, to, maxFlow, transfers)` – turns a `PathfindingResult` into `flowVertices`, `flowEdges`, `streams`, `packedCoordinates` for `hubV2.operateFlowMatrix`.
* `packCoordinates(flowVertices)` / `transformToFlowVertices(transfers)` – lower-level helpers used by `createFlowMatrix` for packing vertex coordinates.
* `getTokenInfoMapFromPath(from, rpcUrl, path)` – fetches token metadata for all tokens/wrappers in a path; feed into other helpers.
* `getWrappedTokensFromPath(path, tokenInfoMap)` – list wrapped tokens present in the path.
* `getExpectedUnwrappedTokenTotals(path, tokenInfoMap)` – estimate ERC1155 amounts recovered after unwrapping wrappers used in the path.
* `replaceWrappedTokensWithAvatars(path, tokenInfoMap)` – rewrite path legs to underlying avatar addresses (for ERC1155 semantics).
* `replaceWrappedTokens(path, unwrappedMap)` – replace wrapped token addresses using a precomputed mapping.
* `shrinkPathValues(path, sink, retainBps)` – scale down flows proportionally (e.g., execute only 70% of a path).
* `assertNoNettedFlowMismatch(path)` – throw if transfers do not net to zero (consistency check).
* `computeNettedFlow(path)` – derive net inflow/outflow per address for diagnostics.

### Error handling & tips

* `maxFlow === 0`: check mutual trust or unwrap static/demurraged balances back to ERC1155.
* Paths needing wrapped balances will fail unless `useWrappedBalances` is true.
