---
icon: user-ninja
---

# SDK Methods

## **1. getAvatar**

Gets an avatar instance by its address.

```typescript
getAvatar: (avatarAddress: string, subscribe?: boolean) => Promise<Avatar>
```

* **Parameters**:
  * `avatarAddress`: The avatar’s wallet address.
  * `subscribe`: Optional, whether to subscribe to avatar events.
* **Returns**: A `Promise` that resolves to an `Avatar` instance.

**Usage Example**:

```typescript
const avatar = await sdk.getAvatar("0x123...abc");
```

***

## **2. acceptInvitation**

Accepts an invitation to join Circles using either CID or Profile.

```typescript
acceptInvitation: (inviter: string, cidV0: string) => Promise<AvatarInterface>;
//or
acceptInvitation: (inviter: string, profile: Profile) => Promise<AvatarInterface>;
```

* **Parameters**:
  * `inviter`: The address of the inviting avatar.
  * `cidV0`: The CIDv0 of the avatar’s metadata (or `profile` data).
* **Returns**: A `Promise` resolving to an `AvatarInterface`.

**Usage Example**:

```typescript
await sdk.acceptInvitation("0xInviterAddress", "QmProfileCID");
```

## **3. registerHuman**

Registers the connected wallet as a human avatar in Circles V1.

```typescript
registerHuman: () => Promise<AvatarInterface>
```

* **Returns**: A `Promise` resolving to an `AvatarInterface`, representing the registered human avatar.

**Usage Example**:

```typescript
const humanAvatar = await sdk.registerHuman();
```

***

## **4. registerOrganization**

Registers the connected wallet as an organization avatar in Circles V1.

```typescript
registerOrganization: () => Promise<AvatarInterface>
```

* **Returns**: A `Promise` resolving to an `AvatarInterface` for the organization avatar.

**Usage Example**:

```typescript
const organizationAvatar = await sdk.registerOrganization();
```

***

## **5. registerOrganizationV2**

Registers the connected wallet as an organization avatar in Circles V2 with profile data.

```typescript
registerOrganizationV2: (profile: Profile) => Promise<AvatarInterface>
```

* **Parameters**:
  * `profile`: A `Profile` object representing the organization’s profile.
* **Returns**: A `Promise` resolving to an `AvatarInterface`.

**Usage Example**:

```typescript
const orgProfile = { name: "OrgName", description: "An example organization." };
const orgAvatarV2 = await sdk.registerOrganizationV2(orgProfile);
```

***

## **6. registerGroupV2**

Registers the connected wallet as a group avatar in Circles V2 with profile data.

```typescript
registerGroupV2: (mint: string, profile: GroupProfile) => Promise<AvatarInterface>
```

* **Parameters**:
  * `mint`: Address of the minting policy contract.
  * `profile`: A `GroupProfile` object containing group information.
* **Returns**: A `Promise` resolving to an `AvatarInterface`.

**Usage Example**:

```typescript
const groupProfile = { name: "GroupName", description: "An example group." };
const groupAvatarV2 = await sdk.registerGroupV2("0xMintAddress", groupProfile);
```

***

## **7. migrateAvatar**

Migrates a V1 avatar and its Circles holdings to V2.

```typescript
migrateAvatar: (avatar: string, profile: Profile, trustRelations?: string[]) => Promise<void>
```

* **Parameters**:
  * `avatar`: The address of the avatar to migrate.
  * `profile`: Profile data of the avatar.
  * `trustRelations`: Optional, a list of trust relations to migrate.
* **Returns**: A `Promise` resolving to `void`.

**Usage Example**:

```typescript
const profile = { name: "John Doe", description: "Human Avatar" };
await sdk.migrateAvatar("0xAvatarAddress", profile);
```

***

## **8. createOrUpdateProfile**

Creates or updates a user profile in Circles.

```typescript
UpdateProfile: (profile: Profile | string) => Promise<ContractTransactionReceipt>
```

* **Parameters**:
  * `profile`: A `Profile` object or a CID string pointing to the profile.
* **Returns**: A `Promise` that resolves to a `ContractTransactionReceipt`.

**Usage Example**:

```typescript
const profileData = { name: "John Doe", description: "Developer" };
const receipt = await sdk.createOrUpdateProfile(profileData);
```

***

## **9. migrateV1Tokens**

Migrates all V1 token holdings of an avatar to V2.

```typescript
migrateV1Tokens: (avatar: string, tokens?: string[]) => Promise<void>
```

* **Parameters**:
  * `avatar`: The avatar whose tokens need to be migrated.
  * `tokens`: Optional list of token addresses.
* **Returns**: A `Promise` resolving to `void`.

**Usage Example**:

```typescript
await sdk.migrateV1Tokens("0xAvatarAddress");
```

***

## **10. getInflationaryWrapper**

Gets an inflationary wrapper for managing tokens.

```typescript
getInflationaryWrapper: (wrapperAddress: string) => Promise<InflationaryCircles>
```

* **Parameters**:
  * `wrapperAddress`: Address of the inflationary wrapper contract.
* **Returns**: A `Promise` resolving to `InflationaryCircles`.

**Usage Example**:

```typescript
const inflationaryWrapper = await sdk.getInflationaryWrapper("0xWrapperAddress");
```

***

## 11. getDemurragedWrapped

This function retrieves a demurrage wrapper, which is used to manage tokens that decrease in value over time (demurrage).

```typescript
getDemurragedWrapper: (wrapperAddress: string) => Promise<DemurrageCircles>
```

### **Parameters:**

* `wrapperAddress`: The address of the demurrage wrapper contract.

**Returns :** A `Promise` that resolves to an instance of `DemurrageCircles`.

**Usage Example:**

```typescript
const demurrageWrapper = await sdk.getDemurragedWrapper("0xWrapperAddress");
```

***

