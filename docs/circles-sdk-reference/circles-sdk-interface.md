---
icon: inbox-full
---

# Circles SDK interface

The Circles SDK provides a high-level interface for interacting with the Circles protocol, focusing on features like user profiles, avatars, token transfers, and contract interactions. It integrates with Circles V1 and V2 hubs, and enables registration, migration, and management of avatars and profiles.

## **Class: `Sdk`**

This class implements the `SdkInterface`, which provides core functionality to interact with the Circles protocol.

The `Sdk` class implements the following interface:

```typescript
import { Avatar } from './avatar';
import { CirclesConfig } from './circlesConfig';
import { Pathfinder } from './v1/pathfinder';
import { AvatarInterface } from './AvatarInterface';
import { Hub as HubV1 } from '@circles-sdk/abi-v1';
import { DemurrageCircles, Hub as HubV2, InflationaryCircles, NameRegistry } from '@circles-sdk/abi-v2';
import { CirclesData, CirclesRpc } from '@circles-sdk/data';
import { GroupProfile, Profile, Profiles } from '@circles-sdk/profiles';
import { ContractTransactionReceipt } from "ethers";
import { SdkContractRunner } from "@circles-sdk/adapter";
import { V2Pathfinder } from "./v2/pathfinderV2";
/**
 * The SDK interface.
 */
interface SdkInterface {
    /**
     * The signer used to sign transactions (connected wallet e.g. MetaMask).
     */
    contractRunner: SdkContractRunner;
    /**
     * The chain specific Circles configuration (contract addresses and rpc endpoints).
     */
    circlesConfig: CirclesConfig;
    /**
     * A configured instance of the CirclesData class, an easy-to-use wrapper around
     * the Circles RPC Query API.
     */
    data: CirclesData;
    /**
     * An instance of the typechain generated Circles V1 Hub contract wrapper.
     */
    v1Hub: HubV1;
    /**
     * An instance of the typechain generated Circles V2 Hub contract wrapper.
     */
    v2Hub?: HubV2;
    /**
     * An instance of the v1 Pathfinder client (necessary for transfers; only available on gnosis chain with v1 Circles at the moment).
     */
    v1Pathfinder?: Pathfinder;
    /**
     * An instance of the v2 Pathfinder client.
     */
    v2Pathfinder?: V2Pathfinder;
    /**
     * Stores and retrieves profiles from the Circles profile service.
     */
    profiles?: Profiles;
    /**
     * Gets an Avatar instance by its address. Fails if the avatar is not signed up at Circles.
     * @param avatarAddress The avatar's address.
     * @returns The Avatar instance.
     */
    getAvatar: (avatarAddress: string) => Promise<Avatar>;
    /**
     * Registers the connected wallet as a human avatar in Circles v1.
     * @returns The Avatar instance.
     */
    registerHuman: () => Promise<AvatarInterface>;
    /**
     * Registers the connected wallet as an organization avatar in Circles v1.
     */
    registerOrganization: () => Promise<AvatarInterface>;
    /**
     * Registers the connected wallet as an organization avatar in Circles v2.
     * @param profile The profile data of the organization.
     */
    registerOrganizationV2: (profile: Profile) => Promise<AvatarInterface>;
    /**
     * Registers the connected wallet as a group avatar in Circles v2.
     * @param mint The address of the minting policy contract to use.
     * @param profile The profile data of the group.
     */
    registerGroupV2: (mint: string, profile: GroupProfile) => Promise<AvatarInterface>;
    /**
     * Migrates a v1 avatar and all its Circles holdings to v2.
     * [[ Currently only works for human avatars. ]]
     * @param avatar The avatar's address.
     * @param profile The profile data of the avatar.
     * @trustRelations An optional list of trust relations to migrate.
     */
    migrateAvatar: (avatar: string, profile: Profile, trustRelations?: string[]) => Promise<void>;
    /**
     * Creates or updates a user profile.
     *
     * @param {Profile | string} profile - Profile object containing user information or a CID pointing to an existing profile.
     * @returns {Promise<ContractTransactionReceipt>} - A promise that resolves to the transaction receipt of the operation.
     */
    createOrUpdateProfile: (profile: Profile | string) => Promise<ContractTransactionReceipt>;
}
/**
 * Wraps a contract runner with its address.
 */
/**
 * The SDK provides a high-level interface to interact with the Circles protocol.
 */
export declare class Sdk implements SdkInterface {
    /**
     * The signer used to sign transactions.
     */
    readonly contractRunner: SdkContractRunner;
    /**
     * The chain specific Circles configuration.
     */
    readonly circlesConfig: CirclesConfig;
    /**
     * The Circles RPC client.
     */
    readonly circlesRpc: CirclesRpc;
    /**
     * The Circles data client.
     */
    readonly data: CirclesData;
    /**
     * The typechain generated V1 hub contract wrapper.
     */
    readonly v1Hub: HubV1;
    /**
     * The typechain generated V2 hub contract wrapper.
     */
    readonly v2Hub?: HubV2;
    /**
     * The typechain generated NameRegistry contract wrapper.
     */
    readonly nameRegistry?: NameRegistry;
    /**
     * The pathfinder client (v1).
     */
    readonly v1Pathfinder?: Pathfinder;
    /**
     * The pathfinder client (v2).
     */
    readonly v2Pathfinder: V2Pathfinder;
    /**
     * The profiles service client.
     */
    readonly profiles?: Profiles;
    /**
     * Creates a new SDK instance.
     * @param contractRunner A contract runner instance and its address.
     * @param config The optional chain specific Circles configuration.
     */
    constructor(contractRunner: SdkContractRunner, config?: CirclesConfig);
    /**
     * Gets an avatar by its address.
     * @param avatarAddress The avatar's address.
     * @param subscribe Whether to subscribe to avatar events.
     * @returns The avatar instance.
     * @throws If the given avatar address is not signed up at Circles.
     */
    getAvatar: (avatarAddress: string, subscribe?: boolean) => Promise<Avatar>;
    /**
     * Creates or updates a profile and registers its CID in the NameRegistry.
     *
     * @param {Profile | string} profile - The profile information or profile ID to be created or updated.
     * @returns {Promise<ContractTransactionReceipt>} - A promise that resolves to the transaction receipt.
     * @throws {Error} - Throws an error if the Profiles service or NameRegistry is not configured,
     *                   or if the transaction fails.
     */
    createOrUpdateProfile: (profile: Profile | string) => Promise<ContractTransactionReceipt>;
    /**
     * Registers the connected wallet as a human avatar.
     * @returns The avatar instance.
     */
    registerHuman: () => Promise<AvatarInterface>;
    /**
     * If you have been invited to Circles, you can accept the invitation and join the Circles network.
     * Specify who invited you and supply the profile you want to use with your new account.
     * @param inviter The address of the avatar that invited you.
     * @param cidV0 The CIDv0 of the avatar's ERC1155 token metadata.
     */
    acceptInvitation(inviter: string, cidV0: string): Promise<AvatarInterface>;
    /**
     * If you have been invited to Circles, you can accept the invitation and join the Circles network.
     * @param inviter The address of the avatar that invited you.
     * @param profile The profile data of the avatar.
     */
    acceptInvitation(inviter: string, profile: Profile): Promise<AvatarInterface>;
    private _registerHuman;
    /**
     * Checks if the profile argument is a string or a Profile object and creates the profile if necessary.
     * If the profile is a string, it must be a CIDv0 string (Qm...).
     * @param profile The profile data or CIDv0 of the avatar.
     * @private
     */
    private createProfileIfNecessary;
    /**
     * Registers the connected wallet as an organization avatar.
     * @returns The avatar instance.
     */
    registerOrganization: () => Promise<AvatarInterface>;
    /**
     * Registers the connected wallet as an organization avatar in Circles v2.
     * @param profile The profile data of the organization.
     */
    registerOrganizationV2: (profile: Profile) => Promise<AvatarInterface>;
    /**
     * Registers the connected wallet as a group avatar in Circles v2.
     * @param mint The address of the minting policy contract to use.
     * @param profile The profile data of the group.
     */
    registerGroupV2: (mint: string, profile: GroupProfile) => Promise<AvatarInterface>;
    private waitForAvatarInfo;
    /**
     * Migrates a v1 avatar and all its Circles holdings to v2.
     * @param avatar The avatar's address.
     * @param profile The profile data of the avatar.
     */
    /**
     * Migrates a v1 avatar and all its Circles holdings to v2.
     * @param avatar The avatar's address.
     * @param profile The profile data of the avatar.
     * @param trustRelations An optional list of trust relations to migrate.
     */
    migrateAvatar: (avatar: string, profile: Profile, trustRelations?: string[]) => Promise<void>;
    /**
     * Migrates all V1 token holdings of an avatar to V2 using batch transactions.
     * @param avatar The avatar whose tokens to migrate.
     * @param tokens An optional list of token addresses to migrate. If not provided, all tokens will be migrated.
     * @param batch An optional batch transaction to add transactions to.
     */
    migrateV1TokensBatch: (avatar: string, tokens?: string[], batch?: any) => Promise<void>;
    /**
     * Migrates all V1 token holdings of an avatar to V2.
     * @param avatar The avatar whose tokens to migrate.
     * @param tokens An optional list of token addresses to migrate. If not provided, all tokens will be migrated.
     */
    migrateV1Tokens: (avatar: string, tokens?: string[]) => Promise<void>;
    getInflationaryWrapper: (wrapperAddress: string) => Promise<InflationaryCircles>;
    getDemurragedWrapper: (wrapperAddress: string) => Promise<DemurrageCircles>;
}
export {};
//# sourceMappingURL=sdk.d.ts.map
```



### **Constructor**

```typescript
constructor(contractRunner: SdkContractRunner, config?: CirclesConfig)
```

* **Parameters**:
  * `contractRunner`: An instance of `SdkContractRunner`, which is responsible for signing transactions.
  * `config`: Optional, an instance of `CirclesConfig`, containing chain-specific configurations (addresses and endpoints).



## **Additional Modules Used**

* `Avatar`: Represents the Circles avatar with methods for interacting with its state.
* `CirclesConfig`: Holds chain-specific configurations such as contract addresses and RPC endpoints.
* `Pathfinder`: Provides routing for token transfers on the Circles network.
* `Profiles`: Manages avatar profiles within Circles.
* `ContractTransactionReceipt`: Used to manage transactions and their receipts on the Ethereum blockchain.
