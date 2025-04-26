---
description: >-
  The CirclesQuery class allows you to execute custom queries against the
  Circles RPC API.
icon: diagram-previous
---

# Using the CirclesQuery Class

The `CirclesData` class (typically accessed via `sdk.data`) returns `CirclesQuery<T>` objects for paged query results. This allows you to execute custom queries against the indexed tables provided by the Circles RPC API and apply filters.

### 1. Write a Query Definition

First, define your query using the `PagedQueryParams` structure, similar to a basic SQL SELECT statement. Key fields include:

*   `namespace`: Distinguishes between tables/views and different Circles versions (e.g., `V_Crc` for V2).
*   `table`: The name of the table to query (e.g., `Avatars`).
*   `columns`: An array of column names to select.
*   `filter`: An array of filter conditions.
*   `sortOrder`: `'ASC'` or `'DESC'`.
*   `limit`: Maximum rows per page (up to 1000).

{% hint style="info" %}
Refer to the [`circles_query` RPC method documentation](https://github.com/aboutcircles/circles-nethermind-plugin/tree/dev?tab=readme-ov-file#circles_query) for a list of available tables and detailed filter options.
{% endhint %}

Here is an example query that reads all avatars of type `group`. You can also filter for `human` or `organization`.

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
For pagination using `queryNextPage()`, you **must** always include `blockNumber`, `transactionIndex`, and `logIndex` in your selected `columns`.
{% endhint %}

### 2. Define a Row Type (Optional)

You can define a TypeScript interface for the expected row structure to get type safety, or use `any`. If defining a custom type, it should extend the `EventRow` type from `@circles-sdk/data` to include the necessary pagination fields (`blockNumber`, `transactionIndex`, `logIndex`).

```typescript
interface MyGroupType extends EventRow {
  avatar: string;
  name: string;
  cidV0Digest?: string;
}
```

### 3. Execute the Query

To execute the query, you need a `CirclesRpc` instance (usually obtained via `sdk.data.circlesRpc` or created manually). Pass this and your query definition to the `CirclesQuery` constructor.

```typescript
// Assuming 'sdk' is initialized
const circlesRpc = sdk.data.circlesRpc;
// Or create manually:
// import { CirclesRpc } from '@circles-sdk/data';
// const circlesRpc = new CirclesRpc('https://rpc.aboutcircles.com/'); // Use appropriate RPC URL

// Create the query instance with your defined type (or <any>)
const query = new CirclesQuery<MyGroupType>(circlesRpc, queryDefinition);
```

Call `queryNextPage()` repeatedly to fetch pages of results. The results for the current page are available via the `currentPage.results` property. The `currentPage` object also contains pagination details (`firstCursor`, `lastCursor`, `limit`, `size`, `sortOrder`).

```typescript
try {
  let hasMore = true;
  while(hasMore) {
    const hasResultsOnPage = await query.queryNextPage();
    if (!hasResultsOnPage) {
      console.log("No more results found.");
      hasMore = false;
    } else {
      const rows = query.currentPage.results;
      console.log(`Fetched page with ${rows.length} results.`);
      rows.forEach(row => console.log(row));
      // Check if there might be more pages based on limit vs size, or specific API response
      if (rows.length < queryDefinition.limit) {
          hasMore = false;
      }
    }
  }
} catch (error) {
    console.error("Error executing query:", error);
}
```

### 4. Add Computed Columns (Optional)

You can extend `CirclesQuery` with computed columns. These are generated client-side based on the retrieved row data. Define a `CalculatedColumn` array where each object has a `name` and a `generator` function.

This example converts the `cidV0Digest` hex string (retrieved from the query) into a standard Base58 CIDv0 string (`Qm...`).

```typescript
// Assuming utility functions hexStringToUint8Array and uint8ArrayToCidV0 are available
// import { hexStringToUint8Array, uint8ArrayToCidV0 } from './your-utils'; // Example import

const calculatedColumns: CalculatedColumn<MyGroupType>[] = [{
  name: 'cidV0', // Name of the new computed column
  generator: async (row: MyGroupType) => {
    if (!row.cidV0Digest) {
      return undefined;
    }
    // Remove '0x' prefix if present and convert hex to bytes, then to CIDv0
    const dataFromHexString = hexStringToUint8Array(row.cidV0Digest.startsWith('0x') ? row.cidV0Digest.substring(2) : row.cidV0Digest);
    return uint8ArrayToCidV0(dataFromHexString);
  }
}];
```

Add the new column name (`cidV0`) to your custom row type:

```typescript
interface MyGroupType extends EventRow {
  avatar: string;
  name: string;
  cidV0Digest?: string;
  cidV0?: string; // Added computed column
}
```

Then, pass the `calculatedColumns` array when creating the `CirclesQuery` instance. The generator function will run for each row retrieved.

```typescript
// Assuming 'circlesRpc', 'queryDefinition', and 'calculatedColumns' are defined

const queryWithComputed = new CirclesQuery<MyGroupType>(circlesRpc, queryDefinition, calculatedColumns);

// Execute the query as shown in Step 3
try {
    const hasResults = await queryWithComputed.queryNextPage();
    if (!hasResults) {
      console.log("The query yielded no results.");
    } else {
      const rows = queryWithComputed.currentPage.results;
      // Each 'row' object will now potentially have the 'cidV0' property
      rows.forEach(row => console.log(row));
    }
} catch (error) {
    console.error("Error executing query with computed columns:", error);
}
```
