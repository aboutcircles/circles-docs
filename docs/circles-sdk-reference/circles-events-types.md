---
icon: calendar-users
---

# Circles Events Types

### Base Event Configuration

All events listed below extend this `CirclesBaseEvent` structure.

* `$event` (`CirclesEventType`): Discriminator for the event kind.
* `blockNumber` (`number`): The block number containing the log.
* `transactionIndex` (`number`): The transaction index within the block.
* `logIndex` (`number`): The log index within the transaction.
* `timestamp` (`number` | _optional_): Timestamp (when provided by the source).
* `transactionHash` (`string` | _optional_): The transaction hash.

***

### 1. Identity & Registration

Events related to the creation and management of Avatars, Groups, and Organizations.

#### CrcV2\_RegisterHuman

Emitted when a new human avatar is registered.

* `$event`: `'CrcV2_RegisterHuman'`
* `avatar` (`string`): The address of the registered human.
* `inviter` (`string`): The address of the user who invited them.

#### CrcV2\_RegisterGroup

Emitted when a new group is registered.

* `$event`: `'CrcV2_RegisterGroup'`
* `group` (`string`): The address of the group.
* `mint` (`string`): The address of the mint policy.
* `treasury` (`string`): The treasury address.
* `name` (`string`): The name of the group.
* `symbol` (`string`): The token symbol.

#### CrcV2\_RegisterOrganization

Emitted when an organization is registered.

* `$event`: `'CrcV2_RegisterOrganization'`
* `organization` (`string`): The address of the organization.
* `name` (`string`): The name of the organization.

#### CrcV2\_InviteHuman

Emitted when an invitation is sent to a potential user.

* `$event`: `'CrcV2_InviteHuman'`
* `inviter` (`string`): The address sending the invite.
* `invited` (`string`): The address being invited.

#### CrcV2\_RegisterShortName

Emitted when a short name (alias) is registered.

* `$event`: `'CrcV2_RegisterShortName'`
* `avatar` (`string`): The avatar address.
* `shortName` (`bigint`): The numeric representation of the short name.
* `nonce` (`bigint`): The nonce used for registration.

#### CrcV2\_Stopped

Emitted when an avatar status is set to stopped.

* `$event`: `'CrcV2_Stopped'`
* `avatar` (`string`): The address of the avatar that has stopped.

***

### 2. Token Operations (Standard & Batch)

Events regarding the movement and minting of tokens (ERC-1155 compliant).

#### CrcV2\_PersonalMint

Emitted when a user mints personal circles.

* `$event`: `'CrcV2_PersonalMint'`
* `human` (`string`): The address of the human minting.
* `amount` (`bigint`): The amount minted.
* `startPeriod` (`bigint`): The start of the minting period.
* `endPeriod` (`bigint`): The end of the minting period.

#### CrcV2\_GroupMint

Emitted when tokens are minted via a Group context.

* `$event`: `'CrcV2_GroupMint'`
* `sender` (`string`): The address initiating the mint.
* `receiver` (`string`): The address receiving the tokens.
* `group` (`string`): The group address.
* `collateral` (`bigint`): The collateral amount involved.
* `amount` (`bigint`): The amount minted.
* `batchIndex` (`number`): The index within the batch operation.

#### CrcV2\_TransferSingle

Emitted when a single token ID is transferred.

* `$event`: `'CrcV2_TransferSingle'`
* `operator` (`string`): The address performing the transfer.
* `from` (`string`): The source address.
* `to` (`string`): The destination address.
* `id` (`bigint`): The token ID.
* `value` (`bigint`): The amount transferred.

#### CrcV2\_TransferBatch

Emitted when multiple token IDs are transferred simultaneously.

* `$event`: `'CrcV2_TransferBatch'`
* `batchIndex` (`number`): The index within the batch operation.
* `operator` (`string`): The address performing the transfer.
* `from` (`string`): The source address.
* `to` (`string`): The destination address.
* `id` (`bigint`): The token ID.
* `value` (`bigint`): The amount transferred.

#### CrcV2\_ApprovalForAll

Emitted when an operator is approved to manage all assets.

* `$event`: `'CrcV2_ApprovalForAll'`
* `account` (`string`): The account owner.
* `operator` (`string`): The address being approved.
* `approved` (`boolean`): Status of approval (`true`/`false`).

#### CrcV2\_TransferSummary

A high-level summary event for transfers.

* `$event`: `'CrcV2_TransferSummary'`
* `from` (`string`): The source address.
* `to` (`string`): The destination address.
* `amount` (`bigint`): The aggregated amount.
* `events` (`string`): Encoded reference to related sub-events.

***

### 3. ERC20 Wrappers

Events related to the wrapping of Circles tokens into standard ERC20 format.

#### CrcV2\_ERC20WrapperDeployed

Emitted when a new ERC20 wrapper contract is deployed.

* `$event`: `'CrcV2_ERC20WrapperDeployed'`
* `avatar` (`string`): The avatar associated with the wrapper.
* `erc20Wrapper` (`string`): The address of the deployed wrapper contract.
* `circlesType` (`number`): The type identifier for the circles.

#### CrcV2\_Erc20WrapperTransfer

Emitted when tokens are transferred via the wrapper.

* `$event`: `'CrcV2_Erc20WrapperTransfer'`
* `tokenAddress` (`string`): The address of the token.
* `from` (`string`): The source address.
* `to` (`string`): The destination address.
* `value` (`bigint`): The value transferred.

***

### 4. Inflation & Demurrage

Events tracking value conversions between inflationary and demurraged states.

#### CrcV2\_DepositInflationary

Emitted when inflationary tokens are deposited.

* `$event`: `'CrcV2_DepositInflationary'`
* `account` (`string`): The user account.
* `amount` (`bigint`): The raw amount deposited.
* `demurragedAmount` (`bigint`): The equivalent demurraged value.

#### CrcV2\_WithdrawInflationary

Emitted when inflationary tokens are withdrawn.

* `$event`: `'CrcV2_WithdrawInflationary'`
* `account` (`string`): The user account.
* `amount` (`bigint`): The raw amount withdrawn.
* `demurragedAmount` (`bigint`): The equivalent demurraged value.

#### CrcV2\_DepositDemurraged

Emitted when demurraged tokens are deposited.

* `$event`: `'CrcV2_DepositDemurraged'`
* `account` (`string`): The user account.
* `amount` (`bigint`): The demurraged amount deposited.
* `inflationaryAmount` (`bigint`): The equivalent inflationary value.

#### CrcV2\_WithdrawDemurraged

Emitted when demurraged tokens are withdrawn.

* `$event`: `'CrcV2_WithdrawDemurraged'`
* `account` (`string`): The user account.
* `amount` (`bigint`): The demurraged amount withdrawn.
* `inflationaryAmount` (`bigint`): The equivalent inflationary value.

#### CrcV2\_DiscountCost

Emitted to record the cost associated with discounting.

* `$event`: `'CrcV2_DiscountCost'`
* `account` (`string`): The user account.
* `id` (`bigint`): The token ID.
* `cost` (`bigint`): The calculated cost.

***

### 5. Trust, Flows & Metadata

Events regarding social graphs, data streams, and IPFS metadata.

#### CrcV2\_Trust

Emitted when a trust relationship is modified.

* `$event`: `'CrcV2_Trust'`
* `truster` (`string`): The entity giving trust.
* `trustee` (`string`): The entity receiving trust.
* `expiryTime` (`bigint`): Timestamp when the trust expires.

#### CrcV2\_StreamCompleted

Emitted when a token stream is finalized.

* `$event`: `'CrcV2_StreamCompleted'`
* `operator` (`string`): The address finalizing the stream.
* `from` (`string`): The source address.
* `to` (`string`): The destination address.
* `id` (`bigint`): The token ID.
* `amount` (`bigint`): The total amount streamed.
* `batchIndex` (`number` | _optional_): The index if part of a batch.

#### CrcV2\_FlowEdgesScopeSingleStarted

Emitted when a single flow edge scope begins.

* `$event`: `'CrcV2_FlowEdgesScopeSingleStarted'`
* `flowEdgeId` (`bigint`): The ID of the flow edge.
* `streamId` (`number`): The ID of the stream.

#### CrcV2\_FlowEdgesScopeLastEnded

Emitted when the last flow edge scope ends.

* `$event`: `'CrcV2_FlowEdgesScopeLastEnded'`
* _(No specific payload properties defined beyond base event)_

#### CrcV2\_UpdateMetadataDigest

Emitted when an avatar's metadata hash is updated.

* `$event`: `'CrcV2_UpdateMetadataDigest'`
* `avatar` (`string`): The avatar address.
* `metadataDigest` (`Uint8Array`): The new metadata hash.

#### CrcV2\_CidV0

Emitted regarding IPFS Content ID (v0) updates.

* `$event`: `'CrcV2_CidV0'`
* `avatar` (`string`): The avatar address.
* `cidV0Digest` (`Uint8Array`): The IPFS CID v0 digest.

#### CrcV2\_URI

Emitted when the URI for a token ID changes.

* `$event`: `'CrcV2_URI'`
* `value` (`string`): The URI string.
* `id` (`bigint`): The token ID.

***

### 6. System & Vaults

#### CrcV2\_CreateVault

Emitted when a new vault is created.

* `$event`: `'CrcV2_CreateVault'`
* `vault` (`string`): The address of the new vault.
* `token` (`string`): The associated token address.
