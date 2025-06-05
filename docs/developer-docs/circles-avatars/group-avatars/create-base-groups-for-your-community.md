---
icon: people-group
---

# Create Base Groups for your community

#### Base Groups are capable of following:

* They can set membership conditions to define who can be part of the group
* They can register short names with a nonce
* They can trust other avatars in batch with conditions

### Base Group Creation Process

To create a base group, you would:

1. Initialize the SDK with the proper configuration
2. Use the `baseGroupFactory` property to call the factory contract's creation methods
3. After creation, get the base group avatar using `sdk.getAvatar()`

```typescript
import { Sdk } from '@circles-sdk/sdk';
import { cidV0ToUint8Array } from '@circles-sdk/utils';
import { ethers } from 'ethers';

// Initialize the SDK
const sdk = new Sdk(contractRunner, config);

// Define the group profile (symbol is required)
const groupProfile = {
  name: "My Base Group",
  symbol: "MBG",
  description: "A base group for community coordination",
  imageUrl: "",             // optional, can be uploaded via SDK
  previewImageUrl: "",      // optional, used for previews
};

// Define base group setup options
const serviceAddress = "0xService...";      // Replace with actual service address
const feeCollection = "0xFeeCollection..."; // Replace with actual treasury address
const initialConditions = [
  "0xAddress1...",
  "0xAddress2..."
  // Add more addresses if needed
];

// Step 1: Create the group profile (CID will be returned)
const profileCID = await sdk.profiles.create(groupProfile);
if (!profileCID) throw new Error("Failed to create profile CID");

// Step 2: Create the base group using the factory
const tx = await sdk.baseGroupFactory.createBaseGroup(
  senderAddress,           // Usually wallet address of the sender
  serviceAddress,
  feeCollection,
  initialConditions,
  groupProfile.name,
  groupProfile.symbol,
  cidV0ToUint8Array(profileCID)  // Convert CID to bytes
);

// Wait for transaction confirmation
const receipt = await tx.wait();

// Step 3: Extract the group address from emitted events
const groupAddress = ethers.stripZerosLeft(receipt.logs[9].topics[1]);

// Step 4: Get the avatar for the created group
const baseGroupAvatar = await sdk.getAvatar(groupAddress.toLowerCase());

console.log("Base group created at:", groupAddress);
console.log("Group avatar:", baseGroupAvatar);

```

### Working with Base Groups

After creating a base group, you can interact with it using the `BaseGroupAvatar` class, which provides methods for:

1. **Membership Management**:
   * `setMembershipCondition(condition, enabled)` - Sets a membership condition
   * `getMembershipConditions()` - Gets the current membership conditions
2. **Trust Management**:
   * `trust(avatar, expiry)` - Trusts another avatar with optional expiry
   * `untrust(avatar)` - Revokes trust from an avatar
   * `trustBatchWithConditions(members, expiry)` - Trusts multiple avatars at once with conditions
3. **Group Administration**:
   * `setOwner(owner)` - Changes the group owner
   * `setService(service)` - Sets the service address
   * `setFeeCollection(feeCollection)` - Sets the fee collection address
   * `registerShortNameWithNonce(nonce)` - Registers a short name for the group
