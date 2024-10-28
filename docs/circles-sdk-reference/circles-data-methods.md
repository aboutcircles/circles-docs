---
icon: plug-circle-check
---

# Circles Data Methods

Circles Data class provides various methods to query and interact with Circles' data, such as balances, transaction history, trust relations, group memberships, and avatar information. It is built around the **Circles RPC** to facilitate communication with the blockchain and retrieve relevant data. The `CirclesData` class exposes methods for both CRCv1 and CRCv2 tokens, trust events, and group information, as well as subscriptions to events.

## 1. **`getTotalBalance`**

Gets the total CRC V1 balance of an address.

```typescript
getTotalBalance(avatar: string, asTimeCircles?: boolean): Promise<string>
```

**Parameters**:

* `avatar`: The address to get the CRC balance for.
* `asTimeCircles` (optional): Return the balance as TimeCircles or not (default is `true`).

**Returns**:\
A `Promise<string>` representing the total balance.

**Usage Example**:

```typescript
const balance = await circlesData.getTotalBalance("0xAvatarAddress");
```

***

## 2. **`getTotalBalanceV2`**

Gets the total CRC V2 balance of an address.

```typescript
getTotalBalanceV2(avatar: string, asTimeCircles?: boolean): Promise<string>
```

**Parameters**:

* `avatar`: The address to get the CRC balance for.
* `asTimeCircles` (optional): Return the balance as TimeCircles or not (default is `true`).

**Returns**:\
A `Promise<string>` representing the total balance.

**Usage Example**:

```typescript
const balanceV2 = await circlesData.getTotalBalanceV2("0xAvatarAddress");
```

***

## 3. **`getTokenBalances`**

Gets the detailed token balances of an address.

```typescript
getTokenBalances(avatar: string): Promise<TokenBalanceRow[]>
```

**Parameters**:

* `avatar`: The address to get the token balances for.

**Returns**:\
A `Promise<TokenBalanceRow[]>` containing the token balances.

**Usage Example**:

```typescript
const balances = await circlesData.getTokenBalances("0xAvatarAddress");
```

***

## 4. **`getTransactionHistory`**

Gets the transaction history of an address (incoming/outgoing transactions and CRC minting).

```typescript
getTransactionHistory(avatar: string, pageSize: number): CirclesQuery<TransactionHistoryRow>
```

**Parameters**:

* `avatar`: The address to get the transaction history for.
* `pageSize`: The maximum number of transactions per page.

**Returns**:\
A `CirclesQuery<TransactionHistoryRow>` object.

**Usage Example**:

```typescript
const history = await circlesData.getTransactionHistory("0xAvatarAddress", 10);
```

***

## 5. **`getTrustRelations`**

Gets the current incoming and outgoing trust relations of an address.

```typescript
getTrustRelations(avatar: string, pageSize: number): CirclesQuery<TrustListRow>
```

**Parameters**:

* `avatar`: The address to get the trust list for.
* `pageSize`: The maximum number of trust relations per page.

**Returns**:\
A `CirclesQuery<TrustListRow>` object.

**Usage Example**:

```typescript
const trustRelations = await circlesData.getTrustRelations("0xAvatarAddress", 10);
```

***

## 6. **`getAggregatedTrustRelations`**

Gets all trust relations of an avatar and groups mutual trust relations together.

```typescript
getAggregatedTrustRelations(avatarAddress: string): Promise<TrustRelationRow[]>
```

**Parameters**:

* `avatarAddress`: The address to get the trust relations for.

**Returns**:\
A `Promise<TrustRelationRow[]>` representing the trust relations.

**Usage Example**:

```typescript
const aggregatedTrust = await circlesData.getAggregatedTrustRelations("0xAvatarAddress");
```

***

## 7. **`getAvatarInfo`**

Gets basic information about an avatar.

```typescript
getAvatarInfo(avatar: string): Promise<AvatarRow | undefined>
```

**Parameters**:

* `avatar`: The address to check.

**Returns**:\
A `Promise<AvatarRow | undefined>` with the avatar info or `undefined` if not found.

**Usage Example**:

```typescript
const avatarInfo = await circlesData.getAvatarInfo("0xAvatarAddress");
```

***

## 8. **`getAvatarInfos`**

Gets basic information about multiple avatars.

```typescript
getAvatarInfos(avatars: string[]): Promise<AvatarRow[]>
```

**Parameters**:

* `avatars`: The addresses to check.

**Returns**:\
A `Promise<AvatarRow[]>` containing avatar information.

**Usage Example**:

```typescript
const avatarInfos = await circlesData.getAvatarInfos(["0xAvatar1", "0xAvatar2"]);
```

***

## 9. **`getTokenInfo`**

Gets the token info for a given token address.

```typescript
getTokenInfo(address: string): Promise<TokenInfoRow | undefined>
```

**Parameters**:

* `address`: The address of the token.

**Returns**:\
A `Promise<TokenInfoRow | undefined>` with the token info or `undefined` if not found.

**Usage Example**:

```typescript
const tokenInfo = await circlesData.getTokenInfo("0xTokenAddress");
```

***

## 10. **`subscribeToEvents`**

Subscribes to Circles events.

```typescript
subscribeToEvents(avatar?: string): Promise<Observable<CirclesEvent>>
```

**Parameters**:

* `avatar` (optional): The avatar to subscribe to. If not provided, all events are subscribed to.

**Returns**:\
A `Promise<Observable<CirclesEvent>>` representing the event stream.

**Usage Example**:

```typescript
const eventStream = await circlesData.subscribeToEvents("0xAvatarAddress");
```

***

## 11. **`getEvents`**

Gets the events for a given avatar in a block range.

```typescript
getEvents(avatar?: string, fromBlock?: number, toBlock?: number, eventTypes?: string[], filters?: FilterPredicate[], sortAscending?: boolean): Promise<CirclesEvent[]>
```

**Parameters**:

* `avatar` (optional): The avatar to get the events for.
* `fromBlock` (optional): The starting block number.
* `toBlock` (optional): The ending block number.
* `eventTypes` (optional): Types of events to filter.
* `filters` (optional): Additional filter criteria.
* `sortAscending` (optional): Whether to sort events in ascending order.

**Returns**:\
A `Promise<CirclesEvent[]>` representing the events.

**Usage Example**:

```typescript
const events = await circlesData.getEvents("0xAvatarAddress", 0, 100, ["Transfer"], [], true);
```

***

## 12. **`getInvitations`**

Gets the invitations sent by an avatar.

```typescript
getInvitations(avatar: string, pageSize: number): CirclesQuery<InvitationRow>
```

**Parameters**:

* `avatar`: The avatar to get the invitations for.
* `pageSize`: The maximum number of invitations per page.

**Returns**:\
A `CirclesQuery<InvitationRow>` object.

**Usage Example**:

```typescript
const invitations = await circlesData.getInvitations("0xAvatarAddress", 10);
```

***

## 13. **`getInvitedBy`**

Gets the avatar that invited the given avatar.

```typescript
getInvitedBy(avatar: string): Promise<string | undefined>
```

**Parameters**:

* `avatar`: The address of the invited avatar.

**Returns**:\
A `Promise<string | undefined>` with the address of the inviting avatar or `undefined` if not found.

**Usage Example**:

```typescript
const inviter = await circlesData.getInvitedBy("0xAvatarAddress");
```

***

## 14. **`findGroups`**

Gets the list of groups.

```typescript
findGroups(pageSize: number, params?: GroupQueryParams): CirclesQuery<GroupRow>
```

**Parameters**:

* `pageSize`: The maximum number of groups per page.
* `params` (optional): Query parameters to filter groups.

**Returns**:\
A `CirclesQuery<GroupRow>` object.

**Usage Example**:

```typescript
const groups = await circlesData.findGroups(10, { name: "ExampleGroup" });
```

***

## 15. **`getGroupMemberships`**

Gets the group memberships of an avatar.

```typescript
getGroupMemberships(avatar: string, pageSize: number): CirclesQuery<GroupMembershipRow>
```

**Parameters**:

* `avatar`: The avatar to get the group memberships for.
* `pageSize`: The maximum number of group memberships per page.

**Returns**:\
A `CirclesQuery<GroupMembershipRow>` object.

**Usage Example**:

```typescript
const memberships = await circlesData.getGroupMemberships("0xAvatarAddress", 10);
```

***

## 16. **`getMetadataCidForAddress`**

Gets the metadata CID for an address.

```typescript
getMetadataCidForAddress(address: string): Promise<string | undefined>
```

**Parameters**:

* `address`: The address to get the metadata CID for.

**Returns**:\
A `Promise<string | undefined>` with the CID or `undefined` if not found.

**Usage Example**:

```typescript
const metadataCid = await circlesData.getMetadataCidForAddress("0xAddress");
```
