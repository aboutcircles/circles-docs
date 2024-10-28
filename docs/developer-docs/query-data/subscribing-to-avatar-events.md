---
icon: diagram-previous
description: >-
  The Circles SDK let's you subscribe to protocol events. Either filtered for an
  avatar or as a complete stream. There is also a way to query all past events
  in a block range.
---

# Subscribing to Avatar events

### Subscribe

To subscribe, you need an initialized CirclesData class.

```typescript
const circlesRpc = new CirclesRpc("https://chiado-rpc.aboutcircles.com");
const data = new CirclesData(circlesRpc);
```

Then call the `subscribeToEvents()` method and supply the address of the avatar to subscribe to:

```typescript
const avatarEvents = await data.subscribeToEvents("0x...");
avatarEvents.subscribe(event => {
    console.log(event);
});
```

If you want to subscribe to all events, call it without parameter:

```typescript
const avatarEvents = await data.subscribeToEvents("0x...");
avatarEvents.subscribe(event => {
    console.log(event);
});
```

### Query past events

If your client missed some events, you can query all events for a specific avatar in a block range.

```typescript
const avatarEvents = await data.getEvents("0x..", 9000000, 10000000);
```

You can omit the last parameter (`toBlock`)  to query from `fromBlock` to the latest block:

```typescript
const avatarEvents = await data.getEvents("0x..", 10000000);
```

### Event types

The above methods yield `CirclesEvent`s. All events have at least the following base properties:

* `$event: CirclesEventType` One of the event types listed below
* `blockNumber: number` In which block the event occurred
* `timestamp?: number` When the event occurred
* `transactionIndex: number` The index of the transaction in the block
* `logIndex: number` The index of the log entry in the transaction
* `transactionHash?: string` The transaction hash

Here's a list of all event types. Please refer to the [source code](https://github.com/aboutcircles/circles-sdk/blob/34160858798e3ec197e405a06d6a199a0bf73412/packages/data/src/events/events.ts) for the event properties.

<pre class="language-typescript"><code class="lang-typescript">export type CirclesEvent =

// CrcV1 Events

<strong>| CrcV1_HubTransfer
</strong>| CrcV1_Signup
| CrcV1_OrganizationSignup
| CrcV1_Trust
| CrcV1_Transfer

<strong>// CrcV2 Events
</strong>
 | CrcV2_InviteHuman
<strong> | CrcV2_PersonalMint
</strong><strong> | CrcV2_RegisterGroup
</strong><strong> | CrcV2_RegisterHuman
</strong><strong> | CrcV2_RegisterOrganization
</strong> | CrcV2_Stopped
<strong> | CrcV2_Trust
</strong> | CrcV2_TransferSingle
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
<strong> | CrcV2_WithdrawInflationary
</strong>
</code></pre>
