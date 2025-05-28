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
// Initialize the SDK  
const sdk = new Sdk(contractRunner, config);  
  
// Create a group profile (with required symbol field)  
const groupProfile: GroupProfile = {  
  name: "My Base Group",  
  symbol: "MBG",  
  description: "A base group for community coordination"  
};  
  
// Create the base group using the factory  
// Note: The exact method name and parameters would depend on the BaseGroupFactory contract interface  
const tx = await sdk.baseGroupFactory.createBaseGroup(  
  mintHandlerAddress,  // Address of the mint handler  
  treasuryAddress,     // Address of the treasury  
  groupProfile.name,  
  groupProfile.symbol,  
  await sdk.createProfileIfNecessary(groupProfile)  
);  
const receipt = await tx.wait();  
  
// Get the group address from the event  
const groupAddress = /* extract from receipt events */;  
  
// Get the avatar instance for the new group  
const baseGroupAvatar = await sdk.getAvatar(groupAddress);
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
