---
description: >-
  The PagedQuery class allows you to execute custom queries against the
  Circles RPC api.
icon: diagram-previous
---

# Utilising PagedQuery Class

Paged results across the SDK (`avatar.history.getTransactions()`, `sdk.groups.getMembers()`, `avatar.group.getGroupMemberships()`, …) are returned as `PagedQuery<TRow>` instances from `@aboutcircles/sdk-rpc`. You can also construct one yourself to query any table in the Circles index with your own columns and filters.

{% hint style="info" %}
Earlier SDK versions exposed a `CirclesQuery<T>` class you instantiated directly. In the current SDK, `CirclesQuery<T>` is only a type, and the class to use is `PagedQuery<TRow>`. Pagination is now cursor-based and handled server-side by the `circles_paginated_query` RPC method.
{% endhint %}

### Write a query

First you'll need to define a query. The basic structure of a query is the same as for a basic SQL select. It has the following fields:

* `namespace`: Used to distinguish between tables and views as well and to tell the tables of the two Circles version apart from each other.
* `table`: The name of the table you want to query.
* `columns`: A list of column names you want to select.
* `filter`: A list of filter conditions that must be met.
* `sortOrder`: `'ASC'` or `'DESC'` (uppercase).
* `limit`: How many rows to return (max: 1000).

{% hint style="info" %}
Check out the documentation of the [`circles_query`rpc method](https://github.com/aboutcircles/circles-nethermind-plugin/tree/dev?tab=readme-ov-file#circles-nethermind-plug-in) for a list of tables.
{% endhint %}

Here is a query that reads all avatars with type `group`. Other avatar types you can try are `human` and `organization`.

```typescript
const queryDefinition: PagedQueryParams = {
  namespace: 'V_Crc',
  table: 'Avatars',
  columns: [
    'blockNumber',
    'transactionIndex',
    'logIndex',
    'avatar',
    'name',
    'cidV0Digest'
  ],
  filter: [
    {
      Type: 'FilterPredicate',
      FilterType: 'Equals',
      Column: 'type',
      Value: 'group'
    }
  ],
  sortOrder: 'ASC',
  limit: 100
};
```

{% hint style="warning" %}
If you want to be able to load the next page (`queryNextPage()`) you must always include the following fields in your query:`blockNumber`, `transactionIndex, logIndex.`
{% endhint %}

### Define a row type

You can define a type for the rows of your query, or just go with `any` if the type doesn't matter.&#x20;

If you want to specify a custom type, it must extend the `EventRow` type. The `EventRow` type contains the `blockNumber`, `transactionIndex` and `logIndex` fields which are required for pagination.

```typescript
interface MyGroupType extends EventRow {
  avatar: string;
  name: string;
  cidV0Digest?: string;
}
```

### Execute the query

To execute the query definition, you'll need a `CirclesRpc` instance. Create one and pass the Circles rpc url to the constructor.

```typescript
import { CirclesRpc, PagedQuery } from '@aboutcircles/sdk-rpc';

const circlesRpc = new CirclesRpc('https://rpc.aboutcircles.com/');
```

Then create a `PagedQuery<MyGroupType>` instance. Note the first constructor argument is `circlesRpc.client`, not the `CirclesRpc` instance itself.

```typescript
const query = new PagedQuery<MyGroupType>(circlesRpc.client, queryDefinition);
```

Call `queryNextPage()` to retrieve the first page of the result set. You can then access the results through the `currentPage` property. This property includes the `results` themselves, along with `limit`, `size`, `sortOrder`, `hasMore`, and `nextCursor`.

```typescript
const hasResults = await query.queryNextPage();
if (!hasResults) {
  console.log("The query yielded no results.");
} else {
  const rows = query.currentPage.results;
  rows.forEach(row => console.log(row));
}
```

To walk the whole result set, keep calling `queryNextPage()` and stop when `hasMore` is false:

```typescript
while (await query.queryNextPage()) {
  query.currentPage.results.forEach(row => console.log(row));
  if (!query.currentPage.hasMore) break;
}
```

### Transform rows

You can post-process every row as it is read by passing a `rowTransformer` callback. Here we convert the value of the previously queried `cidV0Digest` field (which is originally a hex-string) to a CID in `Qm..` format.

```typescript
const query = new PagedQuery<MyGroupType>(
  circlesRpc.client,
  queryDefinition,
  (row) => {
    if (!row.cidV0Digest) {
      return row;
    }

    const dataFromHexString = hexStringToUint8Array(row.cidV0Digest.substring(2));
    return { ...row, cidV0: uint8ArrayToCidV0(dataFromHexString) };
  }
);
```

The new field should be added to the custom type.

```typescript
interface MyGroupType extends EventRow {
  avatar: string;
  name: string;
  cidV0Digest?: string;
  cidV0?: string
}
```

Then you can execute the query just like you did before. The transformer runs for each row in a page.

```typescript
const hasResults = await query.queryNextPage();
if (!hasResults) {
  console.log("The query yielded no results.");
} else {
  const rows = query.currentPage.results;
  rows.forEach(row => console.log(row));
}
```

{% hint style="info" %}
The transformer is synchronous. If you need async enrichment (an IPFS fetch, for example), do it after reading the page rather than inside the transformer.
{% endhint %}
