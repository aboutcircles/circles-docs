---
icon: shovel
description: >-
  The CirclesData class provides an easy-to-use selection of common queries that
  are suitable for most use cases.
---

# Query Circles Data

### Initialization

Most of the previously shown avatar methods internally use the `CirclesData` class with filters for the current avatar address. If you already have a configured `Sdk` instance, you can use the `sdk.data` property to access the class:

```typescript
const data = sdk.data;
```

&#x20;Otherwise you can create an instance like this:

```typescript
const circlesRpc = new CirclesRpc("https://chiado-rpc.aboutcircles.com");
const data = new CirclesData(circlesRpc);
```

### Get avatar info

The `getAvatarInfo(avatar: string): Promise<AvatarRow | undefined>` method finds basic information about an avatar. This includes the signup timestamp, circles version, avatar type (human, organization or group), and token address/id as well as it's profile CID (if any).

This method is useful to check if an avatar exists before using [Broken link](broken-reference "mention").

```typescript
const avatarInfo = await data.getAvatarInfo("0x...");
if (avatarInfo) {
   console.log("Avatar is signed up at Circles");
} else {
   console.log("Avatar is not signed up at Circles");
}
```

### Get token info

The `getTokenInfo(tokenId: string): Promise<TokenInfoRow | undefined>` methods finds basic information about a Circles token. This includes the creation timestamp, circles version, token type (human or group) and the address of the avatar that created the token.

```typescript
const tokenInfo = await data.getTokenInfo("0x...");
if (tokenInfo) {
   console.log("Token is a Circles token");
} else {
   console.log("Token is not a Circles token");
}
```

### Get total balance (v1, v2)

The total Circles balance of an avatar is the sum of all it's personalized and group token holdings. It can be queried with the `getTotalBalance(avatar:string): Promise<string>` method. There is a separate method for each Circles version.

```typescript
const totalBalanceV1 = await data.getTotalBalance("0x...");
const totalBalanceV2 = await data.getTotalBalanceV2("0x...");
```

{% hint style="info" %}
The methods have a second, optional parameter `asTimeCircles?: boolean = true` that controls the return value format. The default value (`true`) returns a floating point `number` as a string, while `false` returns a `bigint` number as a string. If you want to use the value for calculations you need to parse them.
{% endhint %}

### Get detailed token balances (v1, v2)

In contrast to the above method, the `getTokenBalances(avatar: string): Promise<TokenBalanceRow[]>` method gives a detailed overview of an avatar's Circles holdings. As with the method above, this one also exists for both versions of the Circles protocol.

The result row contains the `token`, `balance` and the `tokenOwner`. &#x20;

```typescript
const detailedCirclesBalancesV1 = await data.getTokenBalances("0x...");
const detailedCirclesBalancesV2 = await data.getTokenBalancesV2("0x...");
```

{% hint style="info" %}
The methods have a second, optional parameter `asTimeCircles?: boolean = true` that controls the return value format. The default value (`true`) returns a floating point `number` as a string, while `false` returns a `bigint` number as a string. If you want to use the value for calculations you need to parse them.
{% endhint %}

### Get transaction history

The `getTransactionHistory(avatar: string, pageSize: number): CirclesQuery<TransactionHistoryRow>` method can be used to query all incoming and outgoing Circles transfers from and to an avatar. This includes minting and transfers of personal and group Circles for v1 and v2.

&#x20;The result rows have the following properties:

* `timestamp` When the transaction happened
* `transactionHash`
* `version` If the transaction happened in Circles v1 or v2
* `operator` (the operator that facilitated the transaction - v2 only)
* `from` the sender address
* `to` the receiver address
* `id` in v1: the token address, in v2: the token id
* `value` the transferred raw value for the given version (bigint)
* `timeCircles` a floating point number representation of the `value` for display purposes
* `tokenAddress` an address representation of the numeric tokenid (v2) or the actual erc20 token address of a v1 personal token

```typescript
const query = data.getTransactionHistory("0x...", 25);
const hasResults = await query.queryNextPage();
if (!hasResults) {
   console.log("No transactions yet");
   return;
}

const rows = query.currentPage.results;
rows.forEach(row => console.log(row));
```

The results are ordered in descending order.

### Get trust relations

The `getTrustRelations(avatar: string, pageSize: number): CirclesQuery<TrustListRow>` method can be used to query the effective trust events for an avatar. Already expired or removed trust relations are omitted from the results.&#x20;

```typescript
const trustsQuery = data.getTrustRelations("0x...", 25);
const hasResults = await query.queryNextPage();
if (!hasResults) {
   console.log("No trust events yet");
   return;
}

const rows = query.currentPage.results;
rows.forEach(row => console.log(row));
```

{% hint style="info" %}
The results of this method contain one row per incoming or outgoing event. This is useful when you need to know when a relation was established. However, if you just want to display a contact list you should consider using `getAggregatedTrustRelations(avatarAddress: string): Promise<TrustRelationRow[]>` instead.
{% endhint %}

### Get aggregated trust relations

In contrast to the above method, this method queries all relevant trust events and groups mutual trust events into a single row instead of one for each direction.

The result rows have the following properties:

* `subjectAvatar` The acting avatar
* `relation` The relation between the acting avatar and the one it's related to
* `objectAvatar` The other avatar

The possible relations are: `trusts`, `trustedBy`, `mutuallyTrusts`, and `selfTrusts`. The last one (`selfTrusts`) exists because, in Circles, every avatar trusts itself.

```typescript
const trustRelations = await data.getAggregatedTrustRelations("0x..");
trustRelations.forEach(row => console.log(row));
```

### Find groups

Circles groups have a name and symbol that's stored on-chain. You can use the `findGroups(pageSize: number, params: GroupQueryParams): CirclesQuery<GroupRow>` method to find groups by name or symbol.

The `params` parameter can be used to filter and order the result set by the `name` and `symbol` of a group.

```typescript
export interface GroupQueryParams {
  nameStartsWith?: string;
  symbolStartsWith?: string;
  groupAddressIn?: string[];
  sortBy?: 'age_asc' | 'age_desc' | 'name_asc' | 'name_desc' | 'symbol_asc' | 'symbol_desc';
}
```

Use the method as following:

```typescript
const query = data.findGroups(25, {
  nameStartsWith: "Test",
});

const hasResults = await query.queryNextPage();
if (!hasResults) {
   console.log("No trust events yet");
   return;
}

const rows = query.currentPage.results;
rows.forEach(row => console.log(row));
```

{% hint style="info" %}
If an avatar is member at a group (as defined by a trust relation from the group to the avatar), it's usually eligible to mint tokens of that group.
{% endhint %}

### Get group memberships

You can query the group memberships of an avatar using the `getGroupMemberships(avatar: string, pageSize: number): CirclesQuery<GroupMembershipRow>` method to get a list of all groups an avatar is a member of.

The result rows contain the following properties: `group`, `member`, `expiryTime`.

```typescript
const query = data.getGroupMemberships("0x...", 25);
const hasResults = await query.queryNextPage();
if (!hasResults) {
   console.log("No trust events yet");
   return;
}

const rows = query.currentPage.results;
rows.forEach(row => console.log(row));
```

{% hint style="info" %}
If you want to query the details of the returned groups, you can pass the group addresses into the `groupAddressIn` filter field of the `findGroups()` method.
{% endhint %}

### Get invited users

Avatars can invite others to join Circles. The `getInvitations(avatar: string, pageSize: number): CirclesQuery<InvitationRow>` method returns a list of invitations sent by the specified avatar.

Th e result rows contain the follwing properties: `timestamp`, `transactionHash`, `inviter`, `invited`.

```typescript
const query = data.getInvitations("0x...", 25);
const hasResults = await query.queryNextPage();
if (!hasResults) {
   console.log("No trust events yet");
   return;
}

const rows = query.currentPage.results;
rows.forEach(row => console.log(row));
```

### Get invited by

You can query who invited an other avatar by calling `getInvitedBy(avatar:string): Promise<string|undefined>`. If the avatar wasn't invited, the method returns undefined.

```typescript
const invitedBy = await data.getInvitedBy("0x...");
```

