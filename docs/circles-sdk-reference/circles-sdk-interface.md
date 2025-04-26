---
icon: inbox-full
---

# Circles SDK Interface (`Sdk` Class)

The Circles SDK provides a high-level `Sdk` class for interacting with the Circles protocol. It integrates with V1 and V2 Hub Contracts, enabling avatar registration, profile management, token transfers, and other core functionalities.

## Constructor

```typescript
constructor(contractRunner: SdkContractRunner, config?: CirclesConfig)
```

Creates a new SDK instance.

*   **Parameters**:
    *   `contractRunner` (`SdkContractRunner`): An adapter instance (e.g., `BrowserProviderContractRunner` from `@circles-sdk/adapter-ethers`) responsible for wallet connection and transaction signing.
    *   `config` (`CirclesConfig`, optional): An object containing chain-specific configurations like contract addresses and service endpoints. If omitted, it defaults to the production Gnosis Chain configuration (`circlesConfig` exported from `@circles-sdk/sdk`).

## Public Properties

Instances of the `Sdk` class expose the following public properties:

*   `contractRunner` (`SdkContractRunner`): The adapter instance used for signing transactions.
*   `circlesConfig` (`CirclesConfig`): The configuration object being used by the SDK instance.
*   `circlesRpc` (`CirclesRpc`): The client for interacting with the Circles RPC API.
*   `data` (`CirclesData`): An instance providing convenient methods for querying data via the Circles RPC API.
*   `v1Hub` (`HubV1`): A TypeChain-generated wrapper for the V1 Hub Contract.
*   `v2Hub` (`HubV2` | `undefined`): A TypeChain-generated wrapper for the V2 Hub Contract, if configured.
*   `nameRegistry` (`NameRegistry` | `undefined`): A TypeChain-generated wrapper for the Name Registry contract, if configured.
*   `v1Pathfinder` (`Pathfinder` | `undefined`): The client for the V1 Pathfinder service, if configured.
*   `v2Pathfinder` (`V2Pathfinder` | `undefined`): The client for the V2 Pathfinder service, if configured.
*   `profiles` (`Profiles` | `undefined`): The client for the Profile Service, if configured.

## Public Methods

### `getAvatar`

```typescript
getAvatar(avatarAddress: string, subscribe?: boolean): Promise<Avatar>
```

Retrieves an `Avatar` instance representing a Circles avatar by its address. Throws an error if the address is not registered as a Circles avatar.

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar to retrieve.
    *   `subscribe` (boolean, optional): Whether to subscribe to events for this avatar (default might vary).
*   **Returns**: `Promise<Avatar>` - An instance representing the avatar.

### `createOrUpdateProfile`

```typescript
createOrUpdateProfile(profile: Profile | string): Promise<ContractTransactionReceipt>
```

Creates a new profile or updates an existing one. If `profile` is a `Profile` object, it pins the data to IPFS via the Profile Service and updates the Name Registry. If `profile` is a string, it assumes it's a valid CIDv0 and updates the Name Registry directly. Requires `profiles` and `nameRegistry` to be configured.

*   **Parameters**:
    *   `profile` (`Profile` | string): The profile data object or a pre-pinned CIDv0 string.
*   **Returns**: `Promise<ContractTransactionReceipt>` - The transaction receipt for the Name Registry update.

### `registerHuman`

```typescript
registerHuman(): Promise<AvatarInterface>
```

Registers the connected wallet address as a **V1** Human Avatar. (Note: For V2, use `acceptInvitation`).

*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V1 avatar.

### `acceptInvitation`

```typescript
acceptInvitation(inviter: string, profileData: Profile): Promise<AvatarInterface>
acceptInvitation(inviter: string, cidV0: string): Promise<AvatarInterface>
```

Accepts an invitation to join Circles **V2** as a Human Avatar. Registers the connected wallet address. Requires the inviter's address and profile information.

*   **Parameters**:
    *   `inviter` (string): The address of the avatar that sent the invitation.
    *   `profileData` (`Profile`): The profile data for the new avatar. The SDK will handle pinning via the Profile Service.
    *   `cidV0` (string): Alternatively, a pre-pinned CIDv0 string for the profile data.
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V2 avatar.

### `registerOrganization`

```typescript
registerOrganization(): Promise<AvatarInterface>
```

Registers the connected wallet address as a **V1** Organization Avatar.

*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V1 avatar.

### `registerOrganizationV2`

```typescript
registerOrganizationV2(profile: Profile): Promise<AvatarInterface>
```

Registers the connected wallet address as a **V2** Organization Avatar. Requires profile data.

*   **Parameters**:
    *   `profile` (`Profile`): The profile data for the organization. The SDK handles pinning.
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V2 avatar.

### `registerGroupV2`

```typescript
registerGroupV2(mintPolicy: string, profile: GroupProfile): Promise<AvatarInterface>
```

Registers the connected wallet address as a **V2** Group Avatar. Requires the address of the minting policy contract and the group's profile data.

*   **Parameters**:
    *   `mintPolicy` (string): The address of the minting policy contract for this group.
    *   `profile` (`GroupProfile`): The profile data for the group, including `name` and `symbol`. The SDK handles pinning.
*   **Returns**: `Promise<AvatarInterface>` - An interface representing the newly registered V2 avatar.

### `migrateAvatar`

```typescript
migrateAvatar(avatarAddress: string, profile: Profile, trustRelations?: string[]): Promise<void>
```

Migrates a V1 avatar (specified by `avatarAddress`) to V2. This typically involves registering the avatar in V2 with the provided `profile`, migrating token balances, and optionally re-establishing trust relations. (Note: Currently may only support human avatars).

*   **Parameters**:
    *   `avatarAddress` (string): The address of the V1 avatar to migrate.
    *   `profile` (`Profile`): The profile data to use for the V2 registration.
    *   `trustRelations` (string[], optional): A list of addresses to automatically trust after migration.
*   **Returns**: `Promise<void>`

### `migrateV1Tokens`

```typescript
migrateV1Tokens(avatarAddress: string, tokens?: string[]): Promise<void>
```

Migrates V1 token holdings for a specific avatar to V2.

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar whose V1 tokens should be migrated.
    *   `tokens` (string[], optional): Specific V1 token addresses to migrate. If omitted, attempts to migrate all holdings.
*   **Returns**: `Promise<void>`

### `getInflationaryWrapper`

```typescript
getInflationaryWrapper(wrapperAddress: string): Promise<InflationaryCircles>
```

Gets a TypeChain-generated contract instance for an ERC-20 wrapper (Inflationary type).

*   **Parameters**:
    *   `wrapperAddress` (string): The address of the deployed wrapper contract.
*   **Returns**: `Promise<InflationaryCircles>`

### `getDemurragedWrapper`

```typescript
getDemurragedWrapper(wrapperAddress: string): Promise<DemurrageCircles>
```

Gets a TypeChain-generated contract instance for an ERC-20 wrapper (Demurrage type).

*   **Parameters**:
    *   `wrapperAddress` (string): The address of the deployed wrapper contract.
*   **Returns**: `Promise<DemurrageCircles>`
