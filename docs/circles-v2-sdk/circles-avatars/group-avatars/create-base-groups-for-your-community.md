---
icon: people-group
---

# Create Base Groups for your community

#### Base Groups are capable of following:

* They can set membership conditions to define who can be part of the group
* They can register short names with a nonce
* They can trust avatars individually or in batches with condition checks.

### Create a Base Group

`sdk.register.asGroup` wraps profile pinning, factory deployment, and address extraction.

```ts
import { Sdk } from '@aboutcircles/sdk';

const sdk = new Sdk(
  { rpcUrl: 'https://rpc.aboutcircles.com' },
  runner // ContractRunner tied to your wallet
);

const groupAvatar = await sdk.register.asGroup(
  '0xOwner',
  '0xService',
  '0xFeeCollection',
  ['0xCondition1', '0xCondition2'], // initial membership conditions
  'My Base Group',
  'MBG',
  {
    name: 'My Base Group',
    symbol: 'MBG',
    description: 'A base group for coordination',
    imageUrl: '',        // optional
    previewImageUrl: '', // optional
  }
);

console.log('Base group created at:', groupAvatar.address);
```

The returned instance is a `BaseGroupAvatar`, ready for trust, membership, and admin actions.

### Membership Management

```ts
// List current conditions
const conditions = await groupAvatar.properties.getMembershipConditions();

// Enable/disable a condition
await groupAvatar.setProperties.membershipCondition('0xCondition1', true);
```

### Trust Management

```ts
// Trust avatars (default expiry = max uint96)
await groupAvatar.trust.add(['0xA', '0xB']);

// Untrust
await groupAvatar.trust.remove('0xA');

// Batch trust with membership checks
const oneYear = BigInt(Math.floor(Date.now() / 1000)) + 31536000n;
await groupAvatar.trust.addBatchWithConditions(['0xC', '0xD'], oneYear);
```

### Group Administration

```ts
// Change ownership / service / fee collection
await groupAvatar.setProperties.owner('0xNewOwner');
await groupAvatar.setProperties.service('0xNewService');
await groupAvatar.setProperties.feeCollection('0xNewTreasury');

// Register a short name (nonce required)
await groupAvatar.profile.registerShortName(42);
```

You can also update metadata with `groupAvatar.profile.update({...})`, which pins a new CID and updates on-chain metadata via the group contract.
