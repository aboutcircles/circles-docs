---
icon: calendar-users
---

# Circles Event Types

This document lists the event types emitted by the Circles V1 and V2 protocols, as captured by the SDK.

{% hint style="info" %}
The exact properties available for each event type should be verified by consulting the [SDK source code](https://github.com/aboutcircles/circles-sdk/blob/main/packages/data/src/events/events.ts) or the specific contract ABIs. Properties listed here are based on common patterns but might not be exhaustive or universally present. Properties typed as `Uint8Array` are often represented as hex strings in API responses.
{% endhint %}

### Base Event Properties (`CirclesBaseEvent`)

All Circles events include these common metadata properties:

*   `$event` (`CirclesEventType`): The specific event type name (e.g., `'CrcV2_Trust'`).
*   `blockNumber` (`number`): The block number where the event was logged.
*   `timestamp` (`number`): The approximate timestamp of the block.
*   `transactionIndex` (`number`): Index of the transaction within the block.
*   `logIndex` (`number`): Index of the log entry within the transaction.
*   `transactionHash` (`string`): The hash of the transaction that emitted the event.

***

### V1 Events (`CrcV1_...`)

#### `CrcV1_HubTransfer`

Emitted for token transfers via the V1 Hub (path payments).

*   `from` (`string`): Sender address.
*   `to` (`string`): Receiver address.
*   `amount` (`bigint`): Amount transferred.

***

#### `CrcV1_Signup`

Emitted when a new human user signs up in V1.

*   `user` (`string`): Address of the new user.
*   `token` (`string`): Address of the personal token created for the user.

***

#### `CrcV1_OrganizationSignup`

Emitted when an organization signs up in V1.

*   `organization` (`string`): Address of the organization.

***

#### `CrcV1_Trust`

Emitted when a V1 trust relationship is established or updated.

*   `user` (`string`): Address establishing the trust.
*   `canSendTo` (`string`): Address being trusted.
*   `limit` (`bigint`): Trust limit (typically 100 in V1).

***

#### `CrcV1_Transfer`

Emitted for direct ERC-20 token transfers in V1 (not path payments).

*   `tokenAddress` (`string`): Address of the token contract.
*   `from` (`string`): Sender address.
*   `to` (`string`): Receiver address.
*   `amount` (`bigint`): Amount transferred.

***

### V2 Events (`CrcV2_...`)

#### `CrcV2_InviteHuman`

Emitted when a V2 avatar invites a human address.

*   `inviter` (`string`): Address of the inviter.
*   `invited` (`string`): Address of the invited human.

***

#### `CrcV2_PersonalMint`

Emitted when a human avatar mints personal CRC tokens.

*   `human` (`string`): Address of the human minting tokens.
*   `amount` (`bigint`): Amount of tokens minted.
*   `startPeriod` (`bigint`): Start timestamp of the minting period.
*   `endPeriod` (`bigint`): End timestamp of the minting period.

***

#### `CrcV2_RegisterGroup`

Emitted when a new group is registered.

*   `group` (`string`): Address of the new group avatar.
*   `mint` (`string`): Address of the group's mint policy contract.
*   `treasury` (`string`): Address of the group's treasury contract.
*   `name` (`string`): Name of the group (from profile).
*   `symbol` (`string`): Symbol of the group's token (from profile).

***

#### `CrcV2_RegisterHuman`

Emitted when a human accepts an invitation and registers in V2.

*   `avatar` (`string`): Address of the newly registered human avatar.
*   `inviter` (`string`): Address of the inviter.

***

#### `CrcV2_RegisterOrganization`

Emitted when an organization is registered in V2.

*   `organization` (`string`): Address of the new organization avatar.
*   `name` (`string`): Name of the organization (from profile).

***

#### `CrcV2_Stopped`

Emitted when an avatar manually stops minting (V2).

*   `avatar` (`string`): Address of the avatar that stopped.

***

#### `CrcV2_Trust`

Emitted when a V2 trust relationship is established or updated.

*   `truster` (`string`): Address establishing or updating the trust.
*   `trustee` (`string`): Address being trusted.
*   `expiryTime` (`bigint`): Expiry timestamp of the trust relationship (0 for untrust).

***

#### `CrcV2_TransferSingle`

Emitted for a single ERC-1155 token transfer (V2).

*   `operator` (`string`): Address initiating the transfer (can be `from` or approved operator).
*   `from` (`string`): Sender address.
*   `to` (`string`): Receiver address.
*   `id` (`bigint`): ID of the token being transferred.
*   `value` (`bigint`): Amount of the token transferred.

***

#### `CrcV2_URI`

Emitted when the URI for an ERC-1155 token ID changes (relevant for metadata updates).

*   `value` (`string`): The new URI string.
*   `id` (`bigint`): ID of the token whose URI was updated.

***

#### `CrcV2_ApprovalForAll`

Emitted when an owner grants or revokes approval for an operator to manage all their tokens (ERC-1155 standard).

*   `account` (`string`): Address of the token owner.
*   `operator` (`string`): Address of the operator.
*   `approved` (`boolean`): `true` if approved, `false` if revoked.

***

#### `CrcV2_TransferBatch`

Emitted for a batch ERC-1155 token transfer (V2).

*   `batchIndex` (`number`): Index within the batch operation.
*   `operator` (`string`): Address initiating the transfer.
*   `from` (`string`): Sender address.
*   `to` (`string`): Receiver address.
*   `id` (`bigint`): ID of the token being transferred in this part of the batch.
*   `value` (`bigint`): Amount of the token transferred in this part of the batch.

***

#### `CrcV2_RegisterShortName`

Emitted when a short name is registered via the Name Registry.

*   `avatar` (`string`): Avatar registering the short name.
*   `shortName` (`bigint`): The registered short name.
*   `nonce` (`bigint`): Nonce used for registration.

***

#### `CrcV2_UpdateMetadataDigest`

Emitted when an avatar's metadata digest is updated in the Name Registry (legacy method).

*   `avatar` (`string`): Avatar updating the metadata.
*   `metadataDigest` (`Uint8Array`): The new metadata digest (bytes, often hex string).

***

#### `CrcV2_CidV0`

Emitted when an avatar's profile CIDv0 is updated in the Name Registry.

*   `avatar` (`string`): Avatar updating the CID.
*   `cidV0Digest` (`Uint8Array`): The new CIDv0 digest (bytes, often hex string).

***

#### `CrcV2_StreamCompleted`

(Potentially related to future streaming payment features)

*   `operator` (`string`): Operator address.
*   `from` (`string`): Stream sender.
*   `to` (`string`): Stream receiver.
*   `id` (`bigint`): Token ID.
*   `amount` (`bigint`): Total amount streamed.

***

#### `CrcV2_CreateVault`

Emitted when a new vault contract is created for a group.

*   `vault` (`string`): Address of the new vault contract.
*   `token` (`string`): Address of the group token this vault is associated with.

***

#### `CrcV2_GroupMintSingle` / `CrcV2_GroupMintBatch`

Emitted when a member mints group tokens by providing collateral.

*   (Properties likely include member, group, collateral amount, minted amount - check source)

***

#### `CrcV2_GroupRedeem`

Emitted when a member redeems group tokens to get back collateral.

*   (Properties likely include member, group, redeemed amount, collateral returned - check source)

***

#### `CrcV2_GroupRedeemCollateralReturn` / `CrcV2_GroupRedeemCollateralBurn`

Related to the collateral handling during group token redemption.

*   (Check source for specific properties)

***

#### `CrcV2_DepositDemurraged` / `CrcV2_DepositInflationary`

Related to ERC-20 wrapper interactions (depositing).

*   (Check source for specific properties)

***

#### `CrcV2_WithdrawDemurraged` / `CrcV2_WithdrawInflationary`

Related to ERC-20 wrapper interactions (withdrawing).

*   (Check source for specific properties)

***

#### `CrcV2_Erc20WrapperTransfer` / `CrcV2_Erc20WrapperDeployed`

Related to the deployment and usage of ERC-20 wrapper contracts.

*   (Check source for specific properties)
