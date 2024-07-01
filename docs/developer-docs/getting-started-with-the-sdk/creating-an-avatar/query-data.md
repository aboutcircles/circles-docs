---
description: >-
  Avatars provide easy to use methods to query common values like trust lists
  and balances.
---

# 📝 Query data

### Avatar info

The `avatarInfo` property contains the most important properties of your Avatar like it's `address`, `type`, `tokenId` and `version`.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```

{% hint style="info" %}
Organization Avatars don't have a `tokenId`.
{% endhint %}

### Total balance

This method returns the total Circles balance of an Avatar as `number`.

```typescript
const totalBalance = await avatar.getTotalBalance();
```

### Mintable amount

Human Avatars can mint 24 personal Circles per day. You can call getMintableAmount() to check how much Circles can be minted right now.

```typescript
const mintableAmount = await avatar.getMintableAmount();
```

### Trust relations

Trust relations play a critical role in Circles. A core principle is, that Avatars only accept Circles that they previously trusted. Trust relations and their effects will be explained in greater detail at a later point. For now, you can think of it as a contact list.

```typescript
const trustRelations = await avatar.getTrustRelations();
```

The method returns an array of `TrustRelationRow`s, each with `subjectAvatar`, `relation` and `objectAvatar` attributes. That can be read as:

* 0x111... trusts 0x222...

where the first address is the subject, 'trusts' specifies the relation and the last address is the object. The following relations exist: `trusts`, `trustedBy`, `mutuallyTrusts`.

### Transaction history

Circles transfers, personal and group minting transactions will show up in the avatar's transaction history. You can call `getTransactionHistory(pageSize)` to get a paginated transaction history list.

```typescript
const transactionHistory = await avatar.getTransactionHistory(25);
```

The function returns a `CirclesQuery<T>` object that must be 'unrolled' by calling `queryNextPage()` to get the data. You're going to see more of this later when&#x20;

```typescript
const transactionHistoryQuery = await avatar.getTransactionHistory(25);
const hasData = await transactionHistoryQuery.queryNextPage();
if (hasData) {
    console.log(transactionHistoryQuery.currentPage?.results);
}
```

The result rows contain `from`, `to` and `value` properties as well as (in some cases) the `tokenAddress` of the transferred token. The `currentPage`, additionally to the `results`, has the following fields: `limit`, `size`, `firstCursor`, `lastCursor`, `sortOrder`. You can use these fields e.g. to implement infinite scrolling in your application.&#x20;

{% hint style="warning" %}
The CirclesQuery class is not suitable for classic pagination at the moment because there is no way to get the total result count.
{% endhint %}

### What's next?

On the next page we'll show how you can trust other Avatars, mint personal Circles and transfer them.

Following that, you'll learn how to subscribe to events and how to query the other data that's available from the Circles RPC endpoint.
