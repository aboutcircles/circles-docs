---
icon: diagram-previous
description: >-
  The Circles SDK lets you subscribe to protocol events, either filtered for a
  specific avatar or as a complete stream. You can also query past events within
  a block range.
---

# Subscribing to Avatar Events

### Initialization

Event subscription and querying are handled by the `CirclesData` class, typically accessed via `sdk.data` from your initialized SDK instance.

```typescript
// Assuming 'sdk' is your initialized SDK instance
const data = sdk.data;
```

### 1. Subscribe to Events

The `subscribeToEvents(avatarAddress?: string)` method establishes a subscription to Circles protocol events.

**Subscribe to a specific avatar's events:** Provide the avatar's address.

```typescript
const avatarAddress = "0x..."; // Address of the avatar to monitor
try {
  const avatarSubscription = await data.subscribeToEvents(avatarAddress);
  avatarSubscription.subscribe(event => {
      console.log(`Event for ${avatarAddress}:`, event);
  });
  console.log(`Subscribed to events for ${avatarAddress}`);
  // Remember to handle unsubscription when needed, e.g., avatarSubscription.unsubscribe();
} catch (error) {
    console.error(`Error subscribing to events for ${avatarAddress}:`, error);
}
```

**Subscribe to all events:** Call the method without any parameters.

```typescript
try {
  const allEventsSubscription = await data.subscribeToEvents(); // No address provided
  allEventsSubscription.subscribe(event => {
      console.log("Global Event:", event);
  });
  console.log("Subscribed to all Circles events.");
  // Remember to handle unsubscription when needed, e.g., allEventsSubscription.unsubscribe();
} catch (error) {
    console.error("Error subscribing to all events:", error);
}
```

### 2. Query Past Events

If your client missed events (e.g., due to being offline), you can query historical events for a specific avatar within a given block range using `getEvents(avatarAddress: string, fromBlock: number, toBlock?: number)`.

```typescript
const avatarAddress = "0x...";
const fromBlock = 9000000;
const toBlock = 10000000; // Optional: If omitted, queries up to the latest block

try {
  // Query events within the specified block range
  const pastEvents = await data.getEvents(avatarAddress, fromBlock, toBlock);
  console.log(`Found ${pastEvents.length} events for ${avatarAddress} between blocks ${fromBlock} and ${toBlock || 'latest'}:`);
  pastEvents.forEach(event => console.log(event));

  // Query events from a specific block up to the latest
  // const recentEvents = await data.getEvents(avatarAddress, 10000000);
  // console.log(`Found ${recentEvents.length} recent events for ${avatarAddress}:`);
  // recentEvents.forEach(event => console.log(event));

} catch (error) {
    console.error("Error querying past events:", error);
}
```

### 3. Event Types

The subscription and query methods return `CirclesEvent` objects. All events share these base properties:

*   `$event: CirclesEventType`: The specific type of event (see list below).
*   `blockNumber: number`: The block number where the event occurred.
*   `timestamp?: number`: The approximate timestamp of the block.
*   `transactionIndex: number`: The index of the transaction within the block.
*   `logIndex: number`: The index of the event log within the transaction.
*   `transactionHash?: string`: The hash of the transaction that emitted the event.

Here is a list of possible event types (`CirclesEventType`). For details on the specific properties of each event type, refer to the [SDK source code](https://github.com/aboutcircles/circles-sdk/blob/main/packages/data/src/events/events.ts).

<pre class="language-typescript"><code class="lang-typescript">// Base type for all events
export type CirclesEvent =
  // CrcV1 Events
  | CrcV1_HubTransfer
  | CrcV1_Signup
  | CrcV1_OrganizationSignup
  | CrcV1_Trust
  | CrcV1_Transfer
  // CrcV2 Events
  | CrcV2_InviteHuman
  | CrcV2_PersonalMint
  | CrcV2_RegisterGroup
  | CrcV2_RegisterHuman
  | CrcV2_RegisterOrganization
  | CrcV2_Stopped
  | CrcV2_Trust
  | CrcV2_TransferSingle
  | CrcV2_Erc20WrapperTransfer
  | CrcV2_Erc20WrapperDeployed
  | CrcV2_URI
  | CrcV2_ApprovalForAll
  | CrcV2_TransferBatch
  | CrcV2_RegisterShortName
  | CrcV2_UpdateMetadataDigest
  | CrcV2_CidV0
  | CrcV2_StreamCompleted
  | CrcV2_CreateVault
  | CrcV2_GroupMintSingle
  | CrcV2_GroupMintBatch
  | CrcV2_GroupRedeem
  | CrcV2_GroupRedeemCollateralReturn
  | CrcV2_GroupRedeemCollateralBurn
  | CrcV2_DepositDemurraged
  | CrcV2_DepositInflationary
  | CrcV2_WithdrawDemurraged
  | CrcV2_WithdrawInflationary;
</code></pre>
