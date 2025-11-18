---
icon: user-ninja
---

# SDK Methods

### 1) getAvatar

Gets an avatar instance by address.

```ts
sdk.getAvatar(avatarAddress: string): Promise<Avatar>;
```

* **Parameters:**
  * `avatarAddress` (required): Avatar wallet address.
* **Returns:** `HumanAvatar | OrganisationAvatar | BaseGroupAvatar`
* **Example:**

```ts
const avatar = await sdk.getAvatar('0x123...abc');
```

### 2) register.asHuman

Registers a human avatar; handles invitation redemption automatically.

```ts
sdk.register.asHuman(inviter: string, profile: Profile | string): Promise<HumanAvatar>;
```

* **Parameters:**
  * `inviter` (required): Address of inviting avatar.
  * `profile` (required): Profile object or CID string.
* **Returns:** `HumanAvatar`
* **Example:**

```ts
const human = await sdk.register.asHuman('0xInviter', { name: 'Alice', description: 'Dev' });
```

### 3) register.asOrganization

Registers an organization avatar with profile data.

```ts
sdk.register.asOrganization(profile: Profile | string): Promise<OrganisationAvatar>;
```

* **Parameters:**
  * `profile` (required): Profile object or CID string; must include `name`.
* **Returns:** `OrganisationAvatar`
* **Example:**

```ts
const org = await sdk.register.asOrganization({ name: 'Org', description: 'Example org' });
```

### 4) register.asGroup

Registers a Base Group with profile data.

```ts
sdk.register.asGroup(
  owner: string,
  service: string,
  feeCollection: string,
  initialConditions: string[],
  name: string,
  symbol: string,
  profile: Profile | string
): Promise<BaseGroupAvatar>;
```

* **Parameters:**
  * `owner`, `service`, `feeCollection` (required): Addresses.
  * `initialConditions` (required): Array of condition contract addresses (can be empty).
  * `name` (required, ≤19 chars), `symbol` (required).
  * `profile` (required): Profile object or CID.
* **Returns:** `BaseGroupAvatar`
* **Example:**

```ts
const group = await sdk.register.asGroup(
  owner,
  service,
  feeCollector,
  [],
  'GroupName',
  'GRP',
  { name: 'GroupName', description: 'Example group' }
);
```

### 5) profiles.create (global)

Pins profile data and returns CID.

```ts
sdk.profiles.create(profile: Profile): Promise<string>;
```

* **Parameters:** `profile` (required): Profile object.
* **Returns:** CID string.
* **Example:**

```ts
const cid = await sdk.profiles.create({ name: 'Jane', description: 'Developer' });
```

### 6) avatar.profile.update (avatar-bound)

Pins profile data and updates on-chain metadata digest for that avatar.

```ts
avatar.profile.update(profile: Profile): Promise<string>;
```

* **Parameters:** `profile` (required): Profile object.
* **Returns:** CID string.
* **Example:**

```ts
const cid = await avatar.profile.update({ name: 'Jane', description: 'Updated bio' });
```

### 7) tokens.getInflationaryWrapper

Gets the inflationary ERC20 wrapper address for an avatar’s token (or zero address if undeployed).

```ts
sdk.tokens.getInflationaryWrapper(address: string): Promise<string>;
```

* **Parameters:** `address` (required): Avatar address.
* **Returns:** Wrapper address or zero.
* **Example:**

```ts
const inflWrapper = await sdk.tokens.getInflationaryWrapper('0xAvatar...');
```

### 8) tokens.getDemurragedWrapper

Gets the demurraged ERC20 wrapper address for an avatar’s token (or zero address if undeployed).

```ts
sdk.tokens.getDemurragedWrapper(address: string): Promise<string>;
```

* **Parameters:** `address` (required): Avatar address.
* **Returns:** Wrapper address or zero.
* **Example:**

```ts
const demWrapper = await sdk.tokens.getDemurragedWrapper('0xAvatar...');
```

### 9) Wrapping (avatar.wrap)

Wrap/unwrap ERC1155 CRC into ERC20 wrappers.

* Wrap demurraged:

```ts
avatar.wrap.asDemurraged(avatarAddress: string, amount: bigint): Promise<TransactionReceipt>;
```

* Wrap inflationary:

```ts
avatar.wrap.asInflationary(avatarAddress: string, amount: bigint): Promise<TransactionReceipt>;
```

* Unwrap demurraged:

```ts
avatar.wrap.unwrapDemurraged(wrapperAddress: string, amount: bigint): Promise<TransactionReceipt>;
```

* Unwrap inflationary:

```ts
avatar.wrap.unwrapInflationary(wrapperAddress: string, amount: bigint): Promise<TransactionReceipt>;
```

* **Parameters:**
  * `avatarAddress` / `wrapperAddress` (required)
  * `amount` (required, bigint, atto‑CRC)
* **Returns:** Transaction receipt.
* **Example:**

```ts
await avatar.wrap.asInflationary(avatar.address, BigInt(5e18));
```

### 10) Transfers (avatar.transfer)

* Advanced (pathfinding + unwrap/rewrap):

```ts
avatar.transfer.advanced(
  to: string,
  amount: bigint,
  options?: AdvancedTransferOptions
): Promise<TransactionReceipt>;
```

* Direct (no pathfinding):

```ts
avatar.transfer.direct(
  to: string,
  amount: bigint,
  tokenAddress?: string,
  txData?: Uint8Array
): Promise<TransactionReceipt>;
```

* Max flow helpers:

```ts
avatar.transfer.getMaxAmount(to: string): Promise<bigint>;
avatar.transfer.getMaxAmountAdvanced(to: string, options?: PathfindingOptions): Promise<bigint>;
```

* **Key optional `options`:** `useWrappedBalances`, `fromTokens`, `toTokens`, `excludeFromTokens`, `excludeToTokens`, `simulatedBalances`, `maxTransfers`, `txData`.
* **Example:**

```ts
const receipt = await avatar.transfer.advanced('0xRecipient', BigInt(10e18), { useWrappedBalances: true });
```

### 11) Trust graph (avatar.trust)

```ts
avatar.trust.add(avatarOrList: string | string[], expiry?: bigint): Promise<TransactionReceipt>;
avatar.trust.remove(avatarOrList: string | string[]): Promise<TransactionReceipt>;
avatar.trust.isTrusting(address: string): Promise<boolean>;
avatar.trust.isTrustedBy(address: string): Promise<boolean>;
avatar.trust.getAll(): Promise<AggregatedTrustRelation[]>;
```

* **expiry** optional (defaults to max uint96).
* **Example:**

```ts
await avatar.trust.add('0xFriend'); // indefinite
```

### 12) Balances & history

```ts
avatar.balances.getTotal(): Promise<bigint>;
avatar.balances.getTokenBalances(): Promise<TokenBalanceRow[]>;
avatar.history.getTransactions(limit?: number, sortOrder?: 'ASC' | 'DESC'): PagedQuery<TransactionRow>;
```

* **Example:**

```ts
const tokens = await avatar.balances.getTokenBalances();
```

### 13) Events

```ts
await avatar.subscribeToEvents();
avatar.events.subscribe((event) => console.log(event.$event, event));
avatar.unsubscribeFromEvents();
```

* **Returns:** `subscribeToEvents` resolves when WS subscription is active; `events` is an observable stream of Circles events.
