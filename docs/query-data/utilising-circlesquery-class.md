---
description: >-
  The CirclesQuery class allows you to execute custom queries against the
  Circles RPC api.
icon: diagram-previous
---

# Utilising CirclesQuery Class

The previously shown `CirclesData` class returns `CirclesQuery<T>` objects for all paged query results. You can execute custom queries against all tables in the Circles index and filter with them.

### Write a query

First you'll need to define a query. The basic structure of a query is the same as for a basic SQL select. It has the following fields:

* `namespace`: Used to distinguish between tables and views as well and to tell the tables of the two Circles version apart from each other.
* `table`: The name of the table you want to query.
* `columns`: A list of column names you want to select.
* `filter`: A list of filter conditions that must be met.
* `sortOrder`: Can be 'asc' or 'desc'.
* `limit`: How many rows to return (max: 1000).

{% hint style="info" %}
Check out the documentation of the [circles\_query rpc method](https://github.com/CirclesUBI/circles-nethermind-plugin/tree/dev?tab=readme-ov-file#circles\_query) for a list of tables.
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
If you want to be able to load the next page (`queryNextPage()`) you must always include the following fields in your query:`blockNumber`, `transactionIndex,`` ``logIndex.`
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
const circlesRpc = new CirclesRpc('https://chiado-rpc.aboutcircles.com');
```

Then create a `CirclesQuery<MyGroupType>` instance.

```typescript
const query = new CirclesQuery<MyGroupType>(circlesRpc, queryDefinition);
```

Call `getNextPage()` to retrieve the first page of the result set. You can then access the results through the `currentPage` property. This property includes the `results` themselves, along with `firstCursor`, `lastCursor`, `limit`, `size`, and `sortOrder`.

```typescript
const hasResults = await query.queryNextPage();
if (!hasResults) {
  console.log("The query yielded no results.");
} else {
  const rows = query.currentPage.results;
  rows.forEach(row => console.log(row));
}
```

### Add computed columns

You can extend the CirclesQuery with computed columns. Computed columns are defined by a callback that takes in the row and returns a new value. Here we convert the value of the previously queried `cidV0Digest` field (which is originally a hex-string) to a CID in `Qm..` format.

```typescript
const calculatedColumns = [{
  name: 'cidV0',
  generator: async (row: MyGroupType) => {
    if (!row.cidV0Digest) {
      return undefined;
    }

    const dataFromHexString = hexStringToUint8Array(row.cidV0Digest.substring(2));
    return uint8ArrayToCidV0(dataFromHexString);
  }
}];
```

The new column should be added to the custom type.

```typescript
interface MyGroupType extends EventRow {
  avatar: string;
  name: string;
  cidV0Digest?: string;
  cidV0?: string
}
```

Then you can execute the query just like you did before. The calculated column function will be executed for each row in a page.

```typescript
const query = new CirclesQuery<MyGroupType>(circlesRpc, queryDefinition, calculatedColumns);

const hasResults = await query.queryNextPage();
if (!hasResults) {
  console.log("The query yielded no results.");
} else {
  const rows = query.currentPage.results;
  rows.forEach(row => console.log(row));
}
```
