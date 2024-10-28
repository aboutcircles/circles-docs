---
icon: calendar-users
---

# Circles Events Types

### Base Event: `CirclesBaseEvent`

This is the base type for all Circles events. It contains common metadata for all events.

* **$event**: `CirclesEventType` — The event type, defining which event occurred.
* **blockNumber**: `number` — The block number in which the event was logged.
* **timestamp**: `number` (optional) — The timestamp when the event occurred.
* **transactionIndex**: `number` — Index of the transaction in the block.
* **logIndex**: `number` — Index of the log within the transaction.
* **transactionHash**: `string` (optional) — The hash of the transaction that emitted this event.

***

#### `CrcV1_HubTransfer`

Triggered when a transfer of Circles tokens happens via the Circles Hub.

* **$event**: `'CrcV1_HubTransfer'`
* **from**: `string` (optional) — Address sending the tokens.
* **to**: `string` (optional) — Address receiving the tokens.
* **amount**: `bigint` (optional) — Amount of tokens transferred.

***

#### `CrcV1_Signup`

Triggered when a new user signs up in the Circles system.

* **$event**: `'CrcV1_Signup'`
* **user**: `string` (optional) — Address of the new user.
* **token**: `string` (optional) — The token assigned to the user.

***

#### `CrcV1_OrganizationSignup`

Triggered when an organization signs up.

* **$event**: `'CrcV1_OrganizationSignup'`
* **organization**: `string` (optional) — Address of the organization.

***

#### `CrcV1_Trust`

Emitted when a trust relationship is created.

* **$event**: `'CrcV1_Trust'`
* **canSendTo**: `string` (optional) — The address that can receive tokens from the user.
* **user**: `string` (optional) — The user creating the trust.
* **limit**: `bigint` (optional) — The limit up to which the user can send tokens.

***

#### `CrcV1_Transfer`

Triggered when a token transfer occurs in the Circles V1 system.

* **$event**: `'CrcV1_Transfer'`
* **tokenAddress**: `string` (optional) — Address of the token.
* **from**: `string` (optional) — Address sending the tokens.
* **to**: `string` (optional) — Address receiving the tokens.
* **amount**: `bigint` (optional) — Amount of tokens transferred.

***

#### `CrcV2_InviteHuman`

Triggered when a human is invited to Circles.

* **$event**: `'CrcV2_InviteHuman'`
* **inviter**: `string` (optional) — Address of the inviter.
* **invited**: `string` (optional) — Address of the invited human.

***

#### `CrcV2_PersonalMint`

Triggered when a personal minting event occurs.

* **$event**: `'CrcV2_PersonalMint'`
* **human**: `string` (optional) — Address of the human minting tokens.
* **amount**: `bigint` (optional) — Amount of tokens minted.
* **startPeriod**: `bigint` (optional) — Start of the minting period.
* **endPeriod**: `bigint` (optional) — End of the minting period.

***

#### `CrcV2_RegisterGroup`

Triggered when a group is registered.

* **$event**: `'CrcV2_RegisterGroup'`
* **group**: `string` (optional) — Address of the group.
* **mint**: `string` (optional) — Address of the mint.
* **treasury**: `string` (optional) — Address of the treasury.
* **name**: `string` (optional) — Name of the group.
* **symbol**: `string` (optional) — Symbol for the group.

***

#### `CrcV2_RegisterHuman`

Triggered when a human registers in Circles.

* **$event**: `'CrcV2_RegisterHuman'`
* **avatar**: `string` (optional) — Avatar of the registered human.
* **inviter**: `string` (optional) — Address of the inviter.

***

#### `CrcV2_RegisterOrganization`

Triggered when an organization is registered.

* **$event**: `'CrcV2_RegisterOrganization'`
* **organization**: `string` (optional) — Address of the organization.
* **name**: `string` (optional) — Name of the organization.

***

#### `CrcV2_Stopped`

Triggered when an avatar stops its activity.

* **$event**: `'CrcV2_Stopped'`
* **avatar**: `string` (optional) — Avatar that stopped.

***

#### `CrcV2_Trust`

Triggered when a trust relationship is established in Circles V2.

* **$event**: `'CrcV2_Trust'`
* **truster**: `string` (optional) — The address of the truster.
* **trustee**: `string` (optional) — The address of the trustee.
* **expiryTime**: `bigint` (optional) — Expiry time of the trust relationship.

***

#### `CrcV2_TransferSingle`

Triggered during a single token transfer in Circles V2.

* **$event**: `'CrcV2_TransferSingle'`
* **operator**: `string` (optional) — Address of the operator.
* **from**: `string` (optional) — Address sending the token.
* **to**: `string` (optional) — Address receiving the token.
* **id**: `bigint` (optional) — ID of the token being transferred.
* **value**: `bigint` (optional) — Value of the token transferred.

***

#### `CrcV2_URI`

Triggered when a token's URI is updated.

* **$event**: `'CrcV2_URI'`
* **value**: `string` (optional) — The new URI value.
* **id**: `bigint` (optional) — ID of the token with the updated URI.

***

#### `CrcV2_ApprovalForAll`

Triggered when an account gives or revokes permission to an operator.

* **$event**: `'CrcV2_ApprovalForAll'`
* **account**: `string` (optional) — The account giving or revoking permission.
* **operator**: `string` (optional) — The operator being granted or revoked permission.
* **approved**: `boolean` (optional) — Whether the approval was granted (`true`) or revoked (`false`).

***

#### `CrcV2_TransferBatch`

Triggered during a batch transfer in Circles V2.

* **$event**: `'CrcV2_TransferBatch'`
* **batchIndex**: `number` — Index of the batch.
* **operator**: `string` (optional) — Address of the operator.
* **from**: `string` (optional) — Address sending the tokens.
* **to**: `string` (optional) — Address receiving the tokens.
* **id**: `bigint` (optional) — ID of the token being transferred.
* **value**: `bigint` (optional) — Value of the tokens transferred.

***

#### `CrcV2_RegisterShortName`

Triggered when a short name is registered to an avatar.

* **$event**: `'CrcV2_RegisterShortName'`
* **avatar**: `string` (optional) — Avatar registering the short name.
* **shortName**: `bigint` (optional) — The registered short name.
* **nonce**: `bigint` (optional) — The nonce of the registration.

***

#### `CrcV2_UpdateMetadataDigest`

Triggered when an avatar's metadata digest is updated.

* **$event**: `'CrcV2_UpdateMetadataDigest'`
* **avatar**: `string` (optional) — Avatar updating the metadata.
* **metadataDigest**: `Uint8Array` (optional) — The new metadata digest.

***

#### `CrcV2_CidV0`

Triggered when an avatar's CID (Content Identifier) for metadata is updated.

* **$event**: `'CrcV2_CidV0'`
* **avatar**: `string` (optional) — Avatar updating the CID.
* **cidV0Digest**: `Uint8Array` (optional) — The new CID v0 digest.

***

#### `CrcV2_StreamCompleted`

Triggered when a streaming payment or data transfer is completed.

* **$event**: `'CrcV2_StreamCompleted'`
* **operator**: `string` (optional) — Address of the operator.
* **from**: `string` (optional) — Address sending the streamed payment.
* **to**: `string` (optional) — Address receiving the streamed payment.
* **id**: `bigint` (optional) — ID of the streamed token.
* **amount**: `bigint` (optional) — Total amount streamed.

***

#### `CrcV2_CreateVault`

Triggered when a vault is created.

* **$event**: `'CrcV2_CreateVault'`
* **vault**: `string` (optional) — Address of the vault.
* **token**: `string` (optional) — Address of the token stored in the vault.
