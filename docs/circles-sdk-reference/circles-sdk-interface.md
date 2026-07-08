---
icon: inbox-full
---

# Circles SDK interface

#### Sdk class

Constructor: `new Sdk(config?: CirclesConfig, contractRunner?: ContractRunner)`

* `config` defaults to `circlesConfig[100]` (Gnosis)
* `contractRunner` required for any state-changing call (see ContractRunner below)

Top-level properties:

* `core`: low-level contract wrappers (`@aboutcircles/sdk-core`)
* `rpc`: RPC client (`@aboutcircles/sdk-rpc`)
* `profilesClient`: IPFS profile helper
* `senderAddress`: present when a runner is provided
* `data`: read helpers (see CirclesData)

**Sdk methods**

* `getAvatar(address)` → `HumanAvatar | OrganisationAvatar | BaseGroupAvatar`

**Registration (`sdk.register.*`)**

* `asHuman(inviter, profile)` → `HumanAvatar`
* `asOrganization(profile)` → `OrganisationAvatar`
* `asGroup(owner, service, feeCollection, initialConditions, name, symbol, profile)` → `BaseGroupAvatar`

**Profiles (`sdk.profiles.*`)**

* `create(profile)` → `cid`
* `get(cid)` → `Profile | undefined`

**Tokens (`sdk.tokens.*`)**

* `getInflationaryWrapper(address)` → wrapper address or zero
* `getDemurragedWrapper(address)` → wrapper address or zero
* `getHolders(tokenAddress, limit?, sortOrder?)` → `PagedQuery<TokenHolderRow>`

**Groups (`sdk.groups.*`)**

* `getType(avatar)` → group type (currently unsupported)
* `getMembers(groupAddress, limit?, sortOrder?)` → `PagedQuery<GroupMemberRow>`
* `getCollateral(groupAddress)` → `TokenBalance[]` (group treasury balances)
* `getHolders(groupAddress, limit?)` → `PagedQuery<GroupTokenHolderRow>`

#### CirclesData (`sdk.data`)

Read-only convenience interface:

* `getAvatar(address)` → `AvatarInfo | undefined`
* `getTrustRelations(address)` → `AggregatedTrustRelation[]`
* `getBalances(address)` → `TokenBalance[]`

#### ContractRunner (required for writes)

Minimal contract runner the SDK expects when sending transactions:

* `address` (sender)
* `publicClient` (viem client for reads)
* `init(): Promise<void>`
* `sendTransaction(txs: TransactionRequest[]): Promise<any>` Optional: `estimateGas`, `call`, `resolveName`, `sendBatchTransaction`.

#### Avatar interfaces (shared across Human/Organisation/BaseGroup)

Obtained via `sdk.getAvatar(address)`. All mutate calls require a runner.

**balances**

* `getTotal()` → total CRC
* `getTokenBalances()` → `TokenBalanceRow[]`
* `getTotalSupply()` → BigInt (not implemented for all types)

**trust**

* `add(avatar | avatar[], expiry?)`
* `remove(avatar | avatar[])`
* `isTrusting(address)` / `isTrustedBy(address)`
* `getAll()` → `AggregatedTrustRelation[]`

**profile**

* `get()` → `Profile | undefined`
* `update(profile)` → `cid`
* `updateMetadata(cid)` → tx receipt
* `registerShortName(nonce)` → tx receipt

**history**

* `getTransactions(limit?, sortOrder?)` → `PagedQuery<TransactionRow>`

**transfer**

* `direct(to, amount, tokenAddress?, txData?)` → tx receipt
* `advanced(to, amount, options?)` → tx receipt (pathfinding + unwrap/rewrap)
* `getMaxAmount(to)` / `getMaxAmountAdvanced(to, options?)` → `bigint`

**wrap**

* `asDemurraged(avatarAddress, amount)` → tx receipt
* `asInflationary(avatarAddress, amount)` → tx receipt
* `unwrapDemurraged(wrapperAddress, amount)` → tx receipt
* `unwrapInflationary(wrapperAddress, amount)` → tx receipt

**events**

* `subscribeToEvents()` → sets `avatar.events` observable
* `unsubscribeFromEvents()`
* `events` → observable stream of Circles events

#### Human Avatar specifics

**personalToken**

* `getMintableAmount()` → `{ amount, startPeriod, endPeriod }` (mintable CRC + issuance window)
* `mint()` → tx receipt
* `stop()` → tx receipt (irreversible)

**groupToken (Human / Organisation)**

* `mint(group, amount)` → pathfound transfer to mint handler
* `getMaxMintableAmount(group)` → `bigint`
* `redeem(group, amount)` → tx receipt (automatic redemption)
* `properties.owner(group)` / `properties.mintHandler(group)` / `properties.treasury(group)` / `properties.service(group)` / `properties.feeCollection(group)` / `properties.getMembershipConditions(group)` — read-only (no setters on Human/Organisation)

#### Organisation Avatar specifics

Same `groupToken` surface as HumanAvatar; lacks personal minting.

#### BaseGroup Avatar specifics

BaseGroup avatars do **not** expose a `groupToken` namespace. They manage their own group through no-arg getters and owner-only setters:

**properties (read-only, for this group)**

* `owner()` / `mintHandler()` / `service()` / `feeCollection()`
* `getMembershipConditions()` → `Address[]`

**setProperties (owner-only writes)**

* `owner(newOwner)` → tx receipt
* `service(newService)` → tx receipt
* `feeCollection(newFeeCollection)` → tx receipt
* `membershipCondition(condition, enabled)` → tx receipt

**trust (group-specific addition)**

* `addBatchWithConditions(members, expiry?)` → tx receipt (validates members against membership conditions before trusting)

#### Notes

* Provide a `ContractRunner` for any write call; you can use `SafeBrowserRunner`/`SafeContractRunner` or your own viem-based runner.
* Pathfinding options for transfers mirror `FindPathParams` (`useWrappedBalances`, token include/exclude lists, `maxTransfers`, `simulatedBalances`, etc.).
