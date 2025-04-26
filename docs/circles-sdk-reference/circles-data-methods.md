---
icon: plug-circle-check
---

# Circles Data Methods (`CirclesData` Class)

The `CirclesData` class provides convenient methods to query data indexed by the Circles RPC API, such as balances, transaction history, trust relationships, group memberships, and avatar information. It's typically accessed via `sdk.data`.

## 1. `getTotalBalance`

Gets the total balance of an avatar's *own personal CRC tokens*, checking the appropriate V1 or V2 Hub contract.

```typescript
getTotalBalance(avatarAddress: string, asTimeCircles?: boolean): Promise<string>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar.
    *   `asTimeCircles` (boolean, optional): If `true` (default), returns the balance formatted as a floating-point "TimeCircles" string for display. If `false`, returns the raw `bigint` balance as a string for calculations.
*   **Returns**: `Promise<string>` - The total personal balance.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
try {
  const balance = await data.getTotalBalance(avatarAddress); // TimeCircles format
  const rawBalanceStr = await data.getTotalBalance(avatarAddress, false); // Raw bigint format
  console.log(`Balance (TimeCircles): ${balance}`);
  console.log(`Balance (Raw): ${rawBalanceStr}`);
  // const rawBalanceBigInt = BigInt(rawBalanceStr); // For calculations
} catch (error) {
  console.error("Error getting total balance:", error);
}
```

***

## 2. `getTokenBalances`

Gets a detailed list of *all* token balances held by an avatar, including their own personal CRC and tokens from other avatars they trust. Checks the appropriate V1 or V2 Hub contract.

```typescript
getTokenBalances(avatarAddress: string, asTimeCircles?: boolean): Promise<TokenBalanceRow[]>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar.
    *   `asTimeCircles` (boolean, optional): Controls the format of the `balance` field in the returned rows (default `true` for TimeCircles string, `false` for raw `bigint` string).
*   **Returns**: `Promise<TokenBalanceRow[]>` - An array containing balance details (e.g., `token`, `balance`, `tokenOwner`).

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
try {
  const balances = await data.getTokenBalances(avatarAddress); // TimeCircles format
  console.log("Detailed Balances:", balances);
  // balances.forEach(b => console.log(`${b.tokenOwner}'s token (${b.token}): ${b.balance}`));
} catch (error) {
  console.error("Error getting token balances:", error);
}
```

***

## 3. `getTransactionHistory`

Returns a query object to fetch the transaction history for an avatar (incoming/outgoing transfers, minting) across V1 and V2.

```typescript
getTransactionHistory(avatarAddress: string, pageSize: number): CirclesQuery<TransactionHistoryRow>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar.
    *   `pageSize` (number): Maximum number of transactions per page.
*   **Returns**: `CirclesQuery<TransactionHistoryRow>` - A query object to fetch pages of results.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
const pageSize = 10;
try {
  const historyQuery = data.getTransactionHistory(avatarAddress, pageSize);
  // Use historyQuery.queryNextPage() to fetch pages
  const hasResults = await historyQuery.queryNextPage();
  if (hasResults) {
    console.log("First page of history:", historyQuery.currentPage.results);
  } else {
    console.log("No transaction history found.");
  }
} catch (error) {
  console.error("Error getting transaction history:", error);
}
```

***

## 4. `getTrustRelations` (Events)

Returns a query object to fetch the history of trust *events* (trust/untrust actions) involving an avatar.

```typescript
getTrustRelations(avatarAddress: string, pageSize: number): CirclesQuery<TrustListRow>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar.
    *   `pageSize` (number): Maximum number of events per page.
*   **Returns**: `CirclesQuery<TrustListRow>` - A query object to fetch pages of trust events.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
const pageSize = 10;
try {
  const trustEventsQuery = data.getTrustRelations(avatarAddress, pageSize);
  // Use trustEventsQuery.queryNextPage() to fetch pages
  const hasResults = await trustEventsQuery.queryNextPage();
  if (hasResults) {
    console.log("First page of trust events:", trustEventsQuery.currentPage.results);
  } else {
    console.log("No trust events found.");
  }
} catch (error) {
  console.error("Error getting trust events:", error);
}
```

***

## 5. `getAggregatedTrustRelations` (Current State)

Gets the *current* state of all trust relationships for an avatar, aggregating mutual trust.

```typescript
getAggregatedTrustRelations(avatarAddress: string): Promise<TrustRelationRow[]>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar.
*   **Returns**: `Promise<TrustRelationRow[]>` - An array representing the current trust relationships (e.g., `subjectAvatar`, `relation`, `objectAvatar`).

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
try {
  const aggregatedTrust = await data.getAggregatedTrustRelations(avatarAddress);
  console.log("Current trust state:", aggregatedTrust);
} catch (error) {
  console.error("Error getting aggregated trust relations:", error);
}
```

***

## 6. `getAvatarInfo`

Gets basic information about a single avatar.

```typescript
getAvatarInfo(avatarAddress: string): Promise<AvatarRow | undefined>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address to check.
*   **Returns**: `Promise<AvatarRow | undefined>` - Avatar info or `undefined` if not registered.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
try {
  const avatarInfo = await data.getAvatarInfo(avatarAddress);
  if (avatarInfo) {
    console.log("Avatar Info:", avatarInfo);
  } else {
    console.log("Avatar not found.");
  }
} catch (error) {
  console.error("Error getting avatar info:", error);
}
```

***

## 7. `getAvatarInfos`

Gets basic information for multiple avatar addresses.

```typescript
getAvatarInfos(avatarAddresses: string[]): Promise<AvatarRow[]>
```

*   **Parameters**:
    *   `avatarAddresses` (string[]): An array of addresses to check.
*   **Returns**: `Promise<AvatarRow[]>` - An array containing information for found avatars.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const addresses = ["0xAvatar1", "0xAvatar2", "0xNonExistent"];
try {
  const avatarInfos = await data.getAvatarInfos(addresses);
  console.log("Avatar Infos:", avatarInfos); // Only returns info for registered avatars
} catch (error) {
  console.error("Error getting multiple avatar infos:", error);
}
```

***

## 8. `getTokenInfo`

Gets information for a specific Circles token (V1 address or V2 ID).

```typescript
getTokenInfo(tokenIdOrAddress: string): Promise<TokenInfoRow | undefined>
```

*   **Parameters**:
    *   `tokenIdOrAddress` (string): The V1 token address or V2 token ID.
*   **Returns**: `Promise<TokenInfoRow | undefined>` - Token info or `undefined` if not found.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const tokenIdentifier = "0xTokenAddressOrId";
try {
  const tokenInfo = await data.getTokenInfo(tokenIdentifier);
  if (tokenInfo) {
    console.log("Token Info:", tokenInfo);
  } else {
    console.log("Token not found.");
  }
} catch (error) {
  console.error("Error getting token info:", error);
}
```

***

## 9. `subscribeToEvents`

Subscribes to real-time Circles events, optionally filtered by avatar address.

```typescript
subscribeToEvents(avatarAddress?: string): Promise<Observable<CirclesEvent>>
```

*   **Parameters**:
    *   `avatarAddress` (string, optional): If provided, subscribes only to events involving this avatar. If omitted, subscribes to all events.
*   **Returns**: `Promise<Observable<CirclesEvent>>` - An RxJS Observable stream of events.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress"; // Optional
try {
  const eventStream = await data.subscribeToEvents(avatarAddress);
  const subscription = eventStream.subscribe(event => {
    console.log("Received event:", event);
  });
  // subscription.unsubscribe(); // Call when done listening
} catch (error) {
  console.error("Error subscribing to events:", error);
}
```

***

## 10. `getEvents`

Queries historical events within a block range, with optional filters.

```typescript
getEvents(avatarAddress?: string, fromBlock?: number, toBlock?: number, eventTypes?: string[], filters?: FilterPredicate[], sortAscending?: boolean): Promise<CirclesEvent[]>
```

*   **Parameters**:
    *   `avatarAddress` (string, optional): Filter events involving this avatar.
    *   `fromBlock` (number, optional): Start block number.
    *   `toBlock` (number, optional): End block number (defaults to latest if omitted).
    *   `eventTypes` (string[], optional): Array of event type names (e.g., `"CrcV2_Trust"`) to include.
    *   `filters` (`FilterPredicate[]`, optional): Additional filter criteria (see `CirclesQuery` documentation).
    *   `sortAscending` (boolean, optional): Sort order (default is likely descending).
*   **Returns**: `Promise<CirclesEvent[]>` - An array of matching historical events.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAvatarAddress";
const startBlock = 1000000;
try {
  const events = await data.getEvents(avatarAddress, startBlock, undefined, ["CrcV2_Trust", "CrcV2_TransferSingle"]);
  console.log(`Found ${events.length} Trust/Transfer events since block ${startBlock}:`, events);
} catch (error) {
  console.error("Error getting events:", error);
}
```

***

## 11. `getInvitations`

Returns a query object to fetch invitations sent *by* a specific avatar.

```typescript
getInvitations(avatarAddress: string, pageSize: number): CirclesQuery<InvitationRow>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the inviter avatar.
    *   `pageSize` (number): Maximum number of invitations per page.
*   **Returns**: `CirclesQuery<InvitationRow>` - A query object to fetch pages of invitation events.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const inviterAddress = "0xInviterAddress";
const pageSize = 10;
try {
  const invitationsQuery = data.getInvitations(inviterAddress, pageSize);
  // Use invitationsQuery.queryNextPage() to fetch pages
  const hasResults = await invitationsQuery.queryNextPage();
  if (hasResults) {
    console.log("First page of sent invitations:", invitationsQuery.currentPage.results);
  } else {
    console.log("No invitations sent by this avatar.");
  }
} catch (error) {
  console.error("Error getting invitations:", error);
}
```

***

## 12. `getInvitedBy`

Finds the address of the avatar that invited the given avatar.

```typescript
getInvitedBy(avatarAddress: string): Promise<string | undefined>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the invited avatar.
*   **Returns**: `Promise<string | undefined>` - The inviter's address or `undefined`.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const invitedAvatarAddress = "0xInvitedAvatarAddress";
try {
  const inviter = await data.getInvitedBy(invitedAvatarAddress);
  if (inviter) {
    console.log(`${invitedAvatarAddress} was invited by ${inviter}`);
  } else {
    console.log(`Inviter not found for ${invitedAvatarAddress}.`);
  }
} catch (error) {
  console.error("Error getting inviter:", error);
}
```

***

## 13. `findGroups`

Returns a query object to find Group Avatars, supporting filtering and sorting.

```typescript
findGroups(pageSize: number, params?: GroupQueryParams): CirclesQuery<GroupRow>
```

*   **Parameters**:
    *   `pageSize` (number): Maximum number of groups per page.
    *   `params` (`GroupQueryParams`, optional): Filter/sort criteria (e.g., `nameStartsWith`, `sortBy`).
*   **Returns**: `CirclesQuery<GroupRow>` - A query object to fetch pages of groups.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const pageSize = 10;
try {
  const groupsQuery = data.findGroups(pageSize, { nameStartsWith: "Example", sortBy: 'name_asc' });
  // Use groupsQuery.queryNextPage() to fetch pages
  const hasResults = await groupsQuery.queryNextPage();
  if (hasResults) {
    console.log("First page of found groups:", groupsQuery.currentPage.results);
  } else {
    console.log("No groups found matching criteria.");
  }
} catch (error) {
  console.error("Error finding groups:", error);
}
```

***

## 14. `getGroupMemberships`

Returns a query object to list the groups a specific avatar is a member of.

```typescript
getGroupMemberships(avatarAddress: string, pageSize: number): CirclesQuery<GroupMembershipRow>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the potential member avatar.
    *   `pageSize` (number): Maximum number of memberships per page.
*   **Returns**: `CirclesQuery<GroupMembershipRow>` - A query object to fetch pages of membership details.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const memberAddress = "0xMemberAddress";
const pageSize = 10;
try {
  const membershipsQuery = data.getGroupMemberships(memberAddress, pageSize);
  // Use membershipsQuery.queryNextPage() to fetch pages
  const hasResults = await membershipsQuery.queryNextPage();
  if (hasResults) {
    console.log("First page of group memberships:", membershipsQuery.currentPage.results);
  } else {
    console.log("Avatar is not a member of any groups.");
  }
} catch (error) {
  console.error("Error getting group memberships:", error);
}
```

***

## 15. `getMetadataCidForAddress`

Gets the currently registered profile metadata CID (from Name Registry) for a specific address.

```typescript
getMetadataCidForAddress(address: string): Promise<string | undefined>
```

*   **Parameters**:
    *   `address` (string): The avatar address.
*   **Returns**: `Promise<string | undefined>` - The CIDv0 string or `undefined` if none is set.

**Usage Example**:

```typescript
// Assuming 'data' is sdk.data
const avatarAddress = "0xAddress";
try {
  const metadataCid = await data.getMetadataCidForAddress(avatarAddress);
  if (metadataCid) {
    console.log(`Metadata CID for ${avatarAddress}: ${metadataCid}`);
  } else {
    console.log(`No metadata CID found for ${avatarAddress}.`);
  }
} catch (error) {
  console.error("Error getting metadata CID:", error);
}
```
