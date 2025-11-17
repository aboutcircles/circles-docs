# Find groups and memberships

You can use the RPC helpers on `sdk.rpc.group` to search for Base Groups and to fetch memberships for an avatar. These methods work with cursor-based pagination via `PagedQuery`.

### Initialize RPC Access

```ts
import { Sdk } from '@aboutcircles/sdk';
import { CirclesRpc } from '@aboutcircles/sdk-rpc';

// Preferred: reuse the RPC client from an SDK instance
const sdk = new Sdk({ rpcUrl: 'https://rpc.aboutcircles.com' });
const rpc = sdk.rpc;
```

### Find Groups

Fetch groups with an optional filter and limit:

```ts
const groups = await rpc.group.findGroups(10, {
  nameStartsWith: 'Community', // optional filters
  // ownerIn: ['0xOwner...'],
  // typeIn: ['BaseGroup'],
});

console.log('Retrieved groups:', groups);
```

* `findGroups(limit, params?)` pulls pages under the hood and returns up to `limit` rows.

### Get Group Memberships for an Avatar

`getGroupMemberships` returns a `PagedQuery`; call `queryNextPage()` to iterate.

```ts
const membershipsQuery = rpc.group.getGroupMemberships('0xAvatar', 5);

await membershipsQuery.queryNextPage(); // first page
console.log('Memberships:', membershipsQuery.currentPage?.results);
```

You can continue paging while `currentPage?.hasMore` is true.

### Full Example

```ts
const sdk = new Sdk({ rpcUrl: 'https://rpc.aboutcircles.com' });
const rpc = sdk.rpc;

// Find groups whose names start with "Community"
const groupsResult = await rpc.group.findGroups(10, { nameStartsWith: 'Community' });
console.log('Found groups:', groupsResult);

// Find memberships for a specific avatar
const avatarAddress = '0x123...';
const memberships = rpc.group.getGroupMemberships(avatarAddress, 10);
await memberships.queryNextPage();
console.log('Group memberships:', memberships.currentPage?.results);
```
