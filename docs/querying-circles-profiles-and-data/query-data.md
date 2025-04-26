---
description: >-
  The CirclesData class provides convenient methods for common queries against
  the Circles RPC API.
icon: shovel
---

# Querying Circles Data

### Initialization

Many SDK methods internally use the `CirclesData` class (from `@circles-sdk/data`) to query the Circles RPC API. If you have an initialized `Sdk` instance, you can access this class directly via the `sdk.data` property:

```typescript
// Assuming 'sdk' is your initialized SDK instance
const data = sdk.data;
```

Alternatively, you can instantiate `CirclesData` manually if needed (requires `@circles-sdk/data`):

{% tabs %}
{% tab title="Gnosis Chain (Production)" %}
{% code overflow="wrap" %}
```typescript
// import { CirclesRpc, CirclesData } from '@circles-sdk/data';
const circlesRpc = new CirclesRpc("https://rpc.aboutcircles.com/");
const data = new CirclesData(circlesRpc);
```
{% endcode %}
{% endtab %}

{% tab title="RINGS (Sandbox)" %}
```typescript
// import { CirclesRpc, CirclesData } from '@circles-sdk/data';
const circlesRpc = new CirclesRpc("https://static.94.138.251.148.clients.your-server.de/rpc/");
const data = new CirclesData(circlesRpc);
```
{% endtab %}
{% endtabs %}

### 1. Get Avatar Info

The `getAvatarInfo(avatarAddress: string): Promise<AvatarRow | undefined>` method retrieves basic information about a specific avatar address. This includes signup timestamp, Circles version, avatar type (human, organization, or group), token address/ID, and its profile CID (if set).

```typescript
const avatarAddress = "0x..."; // Address of the avatar to query
try {
  const avatarInfo = await data.getAvatarInfo(avatarAddress);
  if (avatarInfo) {
     console.log("Avatar is signed up at Circles:", avatarInfo);
     // e.g., avatarInfo.type, avatarInfo.version, avatarInfo.cidV0Digest
  } else {
     console.log("Address is not signed up as a Circles avatar.");
  }
} catch (error) {
    console.error("Error getting avatar info:", error);
}
```

### 2. Get Token Info

The `getTokenInfo(tokenId: string): Promise<TokenInfoRow | undefined>` method retrieves basic information about a Circles token ID (V2) or token address (V1). This includes creation timestamp, Circles version, token type (human or group), and the address of the avatar that created the token.

```typescript
const tokenIdOrAddress = "0x..."; // V2 Token ID or V1 Token Address
try {
  const tokenInfo = await data.getTokenInfo(tokenIdOrAddress);
  if (tokenInfo) {
     console.log("Token is a Circles token:", tokenInfo);
     // e.g., tokenInfo.tokenType, tokenInfo.avatar
  } else {
     console.log("Token is not a Circles token.");
  }
} catch (error) {
    console.error("Error getting token info:", error);
}
```

### 3. Get Total Personal Balance

The `getTotalBalance(avatarAddress: string, asTimeCircles?: boolean): Promise<string>` method queries the balance of an avatar's *own personal CRC tokens*. It automatically checks the correct Hub contract (V1 or V2) based on the avatar's version.

```typescript
const avatarAddress = "0x...";
try {
  // Returns balance as 'timeCircles' string by default (floating point)
  const totalBalance = await data.getTotalBalance(avatarAddress);
  console.log(`Total Personal CRC Balance (TimeCircles): ${totalBalance}`);

  // To get the raw bigint balance as a string:
  const rawBalance = await data.getTotalBalance(avatarAddress, false);
  console.log(`Total Personal CRC Balance (Raw BigInt): ${rawBalance}`);
  // Use BigInt(rawBalance) for calculations
} catch (error) {
    console.error("Error getting total personal balance:", error);
}
```

{% hint style="info" %}
The optional `asTimeCircles` parameter (default: `true`) controls the return format. `true` returns a floating-point string suitable for display ("TimeCircles"). `false` returns the raw `bigint` balance as a string, suitable for precise calculations after parsing with `BigInt()`.
{% endhint %}

### 4. Get Detailed Token Balances

The `getTokenBalances(avatarAddress: string, asTimeCircles?: boolean): Promise<TokenBalanceRow[]>` method retrieves a detailed list of *all* token balances held by the specified avatar. This includes their own personal CRC and any tokens from other avatars they trust and hold. It automatically checks the correct Hub contract (V1 or V2).

The result is an array where each row typically contains `token` (address or ID), `balance`, and `tokenOwner`.

```typescript
const avatarAddress = "0x...";
try {
  // Returns balances as 'timeCircles' strings by default
  const detailedBalances = await data.getTokenBalances(avatarAddress);
  console.log("Detailed Balances (TimeCircles):", detailedBalances);

  // To get raw bigint balances as strings:
  const rawDetailedBalances = await data.getTokenBalances(avatarAddress, false);
  console.log("Detailed Balances (Raw BigInt):", rawDetailedBalances);
  // Parse individual balance.balance strings with BigInt() for calculations
} catch (error) {
    console.error("Error getting detailed balances:", error);
}

```

{% hint style="info" %}
Like `getTotalBalance`, the optional `asTimeCircles` parameter controls the balance format in the returned rows.
{% endhint %}

### 5. Get Transaction History

The `getTransactionHistory(avatarAddress: string, pageSize: number): CirclesQuery<TransactionHistoryRow>` method returns a query object to fetch incoming and outgoing Circles transfers for an avatar, including minting events, across both V1 and V2.

The result rows include properties like:

*   `timestamp`: When the transaction occurred.
*   `transactionHash`: The transaction hash.
*   `version`: `1` or `2`, indicating the Circles protocol version.
*   `operator`: The address that facilitated the transaction (V2 only).
*   `from`: The sender address.
*   `to`: The receiver address.
*   `id`: The token address (V1) or token ID (V2).
*   `value`: The raw transferred amount (`bigint` string).
*   `timeCircles`: A floating-point string representation of `value` for display.
*   `tokenAddress`: Address representation of the token (V1 address or V2 derived address).

```typescript
const avatarAddress = "0x...";
const pageSize = 25;
try {
  const historyQuery = data.getTransactionHistory(avatarAddress, pageSize);
  // Use queryNextPage() to fetch pages as shown in the CirclesQuery section
  const hasResults = await historyQuery.queryNextPage();
  if (!hasResults) {
     console.log("No transactions yet for this avatar.");
  } else {
     const rows = historyQuery.currentPage.results;
     console.log(`First page of transactions (${rows.length} results):`);
     rows.forEach(row => console.log(row));
     // You can call historyQuery.queryNextPage() again to get the next page
  }
} catch (error) {
    console.error("Error getting transaction history:", error);
}
```

Results are typically ordered descending by time (most recent first).

### 6. Get Trust Relations (Events)

The `getTrustRelations(avatarAddress: string, pageSize: number): CirclesQuery<TrustListRow>` method returns a query object to fetch trust *events* (trust established/revoked) for an avatar. It omits expired or revoked relationships from the effective state but shows the historical events.

```typescript
const avatarAddress = "0x...";
const pageSize = 25;
try {
  const trustsQuery = data.getTrustRelations(avatarAddress, pageSize);
  const hasResults = await trustsQuery.queryNextPage();
  if (!hasResults) {
     console.log("No trust events found.");
  } else {
     const rows = trustsQuery.currentPage.results;
     console.log(`First page of trust events (${rows.length} results):`);
     rows.forEach(row => console.log(row));
     // Call trustsQuery.queryNextPage() for more pages
  }
} catch (error) {
    console.error("Error getting trust relations events:", error);
}
```

{% hint style="info" %}
This method shows individual trust/untrust events over time. If you only need the current state of trust relationships (who trusts whom), consider using `getAggregatedTrustRelations` instead.
{% endhint %}

### 7. Get Aggregated Trust Relations (Current State)

The `getAggregatedTrustRelations(avatarAddress: string): Promise<TrustRelationRow[]>` method queries all relevant trust events and aggregates them to show the *current* state of trust relationships for an avatar, grouping mutual trust into single entries.

Result rows typically include:

*   `subjectAvatar`: The avatar whose perspective is shown.
*   `relation`: The type of relationship (`trusts`, `trustedBy`, `mutuallyTrusts`, `selfTrusts`).
*   `objectAvatar`: The other avatar in the relationship.

(Note: `selfTrusts` indicates an avatar always trusts itself).

```typescript
const avatarAddress = "0x...";
try {
  const trustRelations = await data.getAggregatedTrustRelations(avatarAddress);
  console.log(`Current trust state for ${avatarAddress}:`);
  trustRelations.forEach(row => console.log(row));
} catch (error) {
    console.error("Error getting aggregated trust relations:", error);
}
```

### 8. Find Groups

The `findGroups(pageSize: number, params?: GroupQueryParams): CirclesQuery<GroupRow>` method returns a query object to find Group Avatars, supporting filtering and sorting.

The optional `params` object allows filtering/sorting:

```typescript
// Interface for GroupQueryParams (may vary slightly in SDK)
export interface GroupQueryParams {
  nameStartsWith?: string;
  symbolStartsWith?: string;
  groupAddressIn?: string[]; // Filter by specific group addresses
  sortBy?: 'age_asc' | 'age_desc' | 'name_asc' | 'name_desc' | 'symbol_asc' | 'symbol_desc';
}
```

Example usage:

```typescript
const pageSize = 25;
try {
  const groupsQuery = data.findGroups(pageSize, {
    nameStartsWith: "Test", // Find groups with names starting with "Test"
    sortBy: 'name_asc'
  });

  const hasResults = await groupsQuery.queryNextPage();
  if (!hasResults) {
     console.log("No groups found matching criteria.");
  } else {
     const rows = groupsQuery.currentPage.results;
     console.log(`Found groups (${rows.length} results):`);
     rows.forEach(row => console.log(row));
     // Call groupsQuery.queryNextPage() for more pages
  }
} catch (error) {
    console.error("Error finding groups:", error);
}
```

### 9. Get Group Memberships

The `getGroupMemberships(avatarAddress: string, pageSize: number): CirclesQuery<GroupMembershipRow>` method returns a query object to list all groups a specific avatar is currently a member of (i.e., groups that trust this avatar).

Result rows typically include `group` (address), `member` (address), and `expiryTime`.

```typescript
const avatarAddress = "0x...";
const pageSize = 25;
try {
  const membershipQuery = data.getGroupMemberships(avatarAddress, pageSize);
  const hasResults = await membershipQuery.queryNextPage();
  if (!hasResults) {
     console.log("Avatar is not a member of any groups.");
  } else {
     const rows = membershipQuery.currentPage.results;
     console.log(`Group memberships (${rows.length} results):`);
     rows.forEach(row => console.log(row));
     // Call membershipQuery.queryNextPage() for more pages
  }
} catch (error) {
    console.error("Error getting group memberships:", error);
}
```

{% hint style="info" %}
To get details of the groups returned here, you can extract the `group` addresses and use them in the `groupAddressIn` filter of the `findGroups()` method.
{% endhint %}

### 10. Get Sent Invitations

The `getInvitations(avatarAddress: string, pageSize: number): CirclesQuery<InvitationRow>` method returns a query object to list invitations sent *by* the specified avatar.

Result rows typically include `timestamp`, `transactionHash`, `inviter`, and `invited`.

```typescript
const avatarAddress = "0x..."; // Address of the inviter
const pageSize = 25;
try {
  const invitationsQuery = data.getInvitations(avatarAddress, pageSize);
  const hasResults = await invitationsQuery.queryNextPage();
  if (!hasResults) {
     console.log("Avatar has not sent any invitations.");
  } else {
     const rows = invitationsQuery.currentPage.results;
     console.log(`Sent invitations (${rows.length} results):`);
     rows.forEach(row => console.log(row));
     // Call invitationsQuery.queryNextPage() for more pages
  }
} catch (error) {
    console.error("Error getting sent invitations:", error);
}
```

### 11. Get Who Invited An Avatar

The `getInvitedBy(avatarAddress: string): Promise<string | undefined>` method queries who invited a specific avatar to join Circles. It returns the inviter's address as a string, or `undefined` if the avatar wasn't invited or the information isn't available.

```typescript
const avatarAddress = "0x..."; // Address of the avatar who was invited
try {
  const invitedBy = await data.getInvitedBy(avatarAddress);
  if (invitedBy) {
    console.log(`${avatarAddress} was invited by ${invitedBy}`);
  } else {
    console.log(`Could not determine who invited ${avatarAddress}.`);
  }
} catch (error) {
    console.error("Error getting inviter:", error);
}
```
