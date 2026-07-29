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
* `circlesConfig`: the resolved config this instance was built with
* `senderAddress`: present when a runner is provided
* `data`: read helpers (see CirclesData)

The IPFS profile helper is internal; use the `sdk.profiles.*` namespace below instead.

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
* `getHolders(tokenAddress, limit?)` → `PagedQuery<TokenHolderRow>` (`limit` defaults to 100)

**Groups (`sdk.groups.*`)**

* `getType(avatar)` → group type — **not implemented**; currently throws `SdkError.unsupportedOperation`
* `getMembers(groupAddress, limit?)` → `PagedQuery<GroupMemberRow>` (`limit` defaults to 100)
* `getCollateral(groupAddress)` → `TokenBalance[]` (group treasury balances)
* `getHolders(groupAddress, limit?)` → `PagedQuery<GroupTokenHolderRow>` (`limit` defaults to 100)

**Referrals (`sdk.referrals.*`)**

Requires `referralsServiceUrl` in `CirclesConfig`; otherwise each call throws a config error.

* `store(privateKey, inviter)` → `void` — key is validated on-chain; `inviter` is self-declared for dashboard visibility
* `retrieve(privateKey)` → referral info (public endpoint, no auth)
* `listMine()` → referrals created by the authenticated user (requires a token provider)

#### CirclesData (`sdk.data`)

Read-only convenience interface:

* `getAvatar(address)` → `AvatarInfo | undefined`
* `getTrustRelations(address)` → `AggregatedTrustRelation[]`
* `getBalances(address)` → `TokenBalance[]`
* `getAllInvitations(address, minimumBalance?)` → invitations involving this address; `minimumBalance` filters out inviters that can no longer cover the fee

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

* `getTransactions(limit?)` → `PagedQuery<TransactionRow>` (`limit` defaults to 50; cursor-based pagination via `queryNextPage()`)

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

**invitation**

Invite flows pick their funding source automatically: free invites first (if the avatar is an eligible Gnosis Pay user), then proxy inviters, then farm quota.

* `getReferralCode()` → `{ transactions, privateKey }` — invite someone who does **not** have a Safe yet; share the private key with them
* `invite(invitee)` → `TransactionRequest[]` — invite an address that already has a Safe but is not registered in Circles
* `getProxyInviters()` → `ProxyInviter[]` — addresses that trust this avatar, are trusted by the invitation module, and hold enough balance (96 CRC per invite)
* `getClaimableFreeInvites()` → `bigint` — free invites claimable as an eligible Gnosis Pay user; `0` when not eligible
* `findInvitePath(proxyInviterAddress?)` → path from this avatar to the invitation module, optionally routed through a specific proxy inviter
* `computeAddress(signer)` → `Address` — deterministic CREATE2 Safe address for a signer (synchronous)
* `generateReferrals(count)` → `{ secrets, signers, transactionReceipt }` — batch referrals via the InvitationFarm
* `getQuota()` → `bigint` — remaining farm invite quota
* `getInvitationFee()` → `bigint` — invitation fee (96 CRC)
* `getInvitationModule()` → `Address` — invitation module address from the farm
* `listReferrals(limit?, offset?)` → `ReferralPreviewList` — referrals created by this avatar (`limit` defaults to 10, `offset` to 0)

**group (memberships)**

* `getGroupMemberships(limit?)` → `PagedQuery<GroupMembershipRow>` — groups this avatar belongs to (`limit` defaults to 50)
* `getGroupMembershipsWithDetails(limit?)` → `GroupRow[]` — same, enriched with name, symbol, owner, treasury, mint handler and member count
* `properties` — alias of `groupToken.properties` below

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

Same `groupToken` surface as HumanAvatar; lacks personal minting and the `invitation` namespace. Its `group` namespace only aliases `groupToken.properties` — the membership queries (`getGroupMemberships`) are Human-only.

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

* Base Groups are the group type the SDK models directly. Other, custom group implementations are possible on the protocol — they use their own mint policies and are not covered by the `BaseGroupAvatar` surface described here.
* Provide a `ContractRunner` for any write call; you can use `SafeBrowserRunner`/`SafeContractRunner` or your own viem-based runner.
* Pathfinding options for transfers mirror `FindPathParams` (`useWrappedBalances`, token include/exclude lists, `maxTransfers`, `simulatedBalances`, etc.).
