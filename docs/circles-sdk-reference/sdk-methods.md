---
icon: user-ninja
---

# SDK Methods (`Sdk` Class)

This section details the public methods available on an instance of the `Sdk` class.

## 1. `getAvatar`

Retrieves an `Avatar` instance representing a Circles avatar by its address. Throws an error if the address is not registered.

```typescript
getAvatar(avatarAddress: string, subscribe?: boolean): Promise<Avatar>
```

*   **Parameters**:
    *   `avatarAddress` (string): The avatar's address.
    *   `subscribe` (boolean, optional): Whether to subscribe to events for this avatar.
*   **Returns**: `Promise<Avatar>` - An instance representing the avatar.

**Usage Example**:

```typescript
try {
  const avatar = await sdk.getAvatar("0x123...abc");
  console.log("Avatar found:", avatar.avatarInfo);
} catch (error) {
  console.error("Error getting avatar:", error); // Likely address not registered
}
```

***

## 2. `acceptInvitation`

Accepts an invitation to join Circles V2 as a Human Avatar, registering the connected wallet address. Requires the inviter's address and profile information (either as data or a pre-pinned CID).

```typescript
acceptInvitation(inviter: string, cidV0: string): Promise<AvatarInterface>;
acceptInvitation(inviter: string, profile: Profile): Promise<AvatarInterface>;
```

*   **Parameters**:
    *   `inviter` (string): The address of the inviting avatar.
    *   `cidV0` (string): The pre-pinned CIDv0 of the avatar's profile metadata.
    *   `profile` (`Profile`): Alternatively, the profile data object (SDK handles pinning).
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V2 avatar.

**Usage Examples**:

```typescript
// Using a pre-pinned CID
try {
  const avatar1 = await sdk.acceptInvitation("0xInviterAddress", "QmProfileCID");
  console.log("Invitation accepted (CID):", avatar1);
} catch (error) {
  console.error("Error accepting invitation (CID):", error);
}

// Using profile data object
try {
  const profileData = { name: "New User" };
  const avatar2 = await sdk.acceptInvitation("0xInviterAddress", profileData);
  console.log("Invitation accepted (Profile Data):", avatar2);
} catch (error) {
  console.error("Error accepting invitation (Profile Data):", error);
}
```

***

## 3. `registerHuman`

Registers the connected wallet address as a **V1** Human Avatar.

```typescript
registerHuman(): Promise<AvatarInterface>
```

*   **Returns**: `Promise<AvatarInterface>` - An interface representing the registered V1 Human Avatar.

**Usage Example**:

```typescript
try {
  const humanAvatar = await sdk.registerHuman();
  console.log("V1 Human Avatar registered:", humanAvatar);
} catch (error) {
  console.error("Error registering V1 Human Avatar:", error);
}
```

***

## 4. `registerOrganization`

Registers the connected wallet address as a **V1** Organization Avatar.

```typescript
registerOrganization(): Promise<AvatarInterface>
```

*   **Returns**: `Promise<AvatarInterface>` - An interface representing the registered V1 Organization Avatar.

**Usage Example**:

```typescript
try {
  const orgAvatar = await sdk.registerOrganization();
  console.log("V1 Organization Avatar registered:", orgAvatar);
} catch (error) {
  console.error("Error registering V1 Organization Avatar:", error);
}
```

***

## 5. `registerOrganizationV2`

Registers the connected wallet address as a **V2** Organization Avatar. Requires profile data.

```typescript
registerOrganizationV2(profile: Profile): Promise<AvatarInterface>
```

*   **Parameters**:
    *   `profile` (`Profile`): The profile data object for the organization (SDK handles pinning).
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the registered V2 Organization Avatar.

**Usage Example**:

```typescript
const orgProfile = { name: "OrgName", description: "An example organization." };
try {
  const orgAvatarV2 = await sdk.registerOrganizationV2(orgProfile);
  console.log("V2 Organization Avatar registered:", orgAvatarV2);
} catch (error) {
  console.error("Error registering V2 Organization Avatar:", error);
}
```

***

## 6. `registerGroupV2`

Registers the connected wallet address as a **V2** Group Avatar. Requires the mint policy address and group profile data.

```typescript
registerGroupV2(mintPolicy: string, profile: GroupProfile): Promise<AvatarInterface>
```

*   **Parameters**:
    *   `mintPolicy` (string): Address of the minting policy contract for the group.
    *   `profile` (`GroupProfile`): The profile data object for the group (SDK handles pinning).
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the registered V2 Group Avatar.

**Usage Example**:

```typescript
const groupProfile = { name: "GroupName", symbol: "GRP", description: "An example group." };
const mintPolicyAddress = "0xMintPolicyAddress"; // Use standard or custom policy
try {
  const groupAvatarV2 = await sdk.registerGroupV2(mintPolicyAddress, groupProfile);
  console.log("V2 Group Avatar registered:", groupAvatarV2);
} catch (error) {
  console.error("Error registering V2 Group Avatar:", error);
}
```

***

## 7. `migrateAvatar`

Migrates a V1 avatar (human only currently) and its Circles holdings to V2. Registers the avatar in V2, migrates tokens, and optionally re-establishes trust.

```typescript
migrateAvatar(avatarAddress: string, profile: Profile, trustRelations?: string[]): Promise<void>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the V1 avatar to migrate.
    *   `profile` (`Profile`): Profile data for the V2 registration.
    *   `trustRelations` (string[], optional): List of addresses to trust in V2 after migration.
*   **Returns**: `Promise<void>`

**Usage Example**:

```typescript
const profile = { name: "John Doe", description: "Migrated Human Avatar" };
const v1AvatarAddress = "0xV1AvatarAddress";
try {
  await sdk.migrateAvatar(v1AvatarAddress, profile);
  console.log(`Avatar ${v1AvatarAddress} migration initiated.`);
} catch (error) {
  console.error(`Error migrating avatar ${v1AvatarAddress}:`, error);
}
```

***

## 8. `createOrUpdateProfile`

Creates or updates an avatar's profile by pinning data to IPFS (if an object is provided) and updating the on-chain Name Registry reference.

```typescript
createOrUpdateProfile(profile: Profile | string): Promise<ContractTransactionReceipt>
```

*   **Parameters**:
    *   `profile` (`Profile` | string): A `Profile` data object or a pre-pinned CIDv0 string.
*   **Returns**: `Promise<ContractTransactionReceipt>` - The transaction receipt for the Name Registry update.

**Usage Example**:

```typescript
const profileData = { name: "John Doe Updated", description: "Senior Developer" };
try {
  const receipt = await sdk.createOrUpdateProfile(profileData);
  console.log("Profile update transaction successful:", receipt);
} catch (error) {
  console.error("Error creating/updating profile:", error);
}
```

***

## 9. `migrateV1Tokens`

Migrates V1 token holdings for a specific avatar to V2.

```typescript
migrateV1Tokens(avatarAddress: string, tokens?: string[]): Promise<void>
```

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar whose V1 tokens need migration.
    *   `tokens` (string[], optional): Specific V1 token addresses to migrate. If omitted, attempts to migrate all holdings.
*   **Returns**: `Promise<void>`

**Usage Example**:

```typescript
const v1AvatarAddress = "0xV1AvatarAddress";
try {
  await sdk.migrateV1Tokens(v1AvatarAddress);
  console.log(`V1 token migration initiated for ${v1AvatarAddress}.`);
} catch (error) {
  console.error(`Error migrating V1 tokens for ${v1AvatarAddress}:`, error);
}
```

***

## 10. `getInflationaryWrapper`

Gets a TypeChain-generated contract instance for an Inflationary ERC-20 wrapper contract.

```typescript
getInflationaryWrapper(wrapperAddress: string): Promise<InflationaryCircles>
```

*   **Parameters**:
    *   `wrapperAddress` (string): Address of the deployed inflationary wrapper contract.
*   **Returns**: `Promise<InflationaryCircles>` - An ethers contract instance.

**Usage Example**:

```typescript
const wrapperAddress = "0xInflationaryWrapperAddress";
try {
  const inflationaryWrapper = await sdk.getInflationaryWrapper(wrapperAddress);
  console.log("Inflationary Wrapper instance:", inflationaryWrapper);
  // Now you can call methods on inflationaryWrapper
} catch (error) {
  console.error("Error getting inflationary wrapper:", error);
}
```

***

## 11. `getDemurragedWrapper`

Gets a TypeChain-generated contract instance for a Demurrage ERC-20 wrapper contract, used to manage tokens that decrease in value over time.

```typescript
getDemurragedWrapper(wrapperAddress: string): Promise<DemurrageCircles>
```

*   **Parameters**:
    *   `wrapperAddress` (string): The address of the deployed demurrage wrapper contract.
*   **Returns**: `Promise<DemurrageCircles>` - An ethers contract instance.

**Usage Example**:

```typescript
const wrapperAddress = "0xDemurrageWrapperAddress";
try {
  const demurrageWrapper = await sdk.getDemurragedWrapper(wrapperAddress);
  console.log("Demurrage Wrapper instance:", demurrageWrapper);
  // Now you can call methods on demurrageWrapper
} catch (error) {
  console.error("Error getting demurrage wrapper:", error);
}
```

***
