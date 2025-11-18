---
icon: calendar-users
---

# Circles Events Types

Event payloads emitted by Circles contracts and surfaced through `avatar.events` or RPC subscriptions. Each event extends the base metadata below.

### Base type: `CirclesBaseEvent`

* `$event: CirclesEventType` – discriminator for the event kind.
* `blockNumber: number` – block containing the log.
* `transactionIndex: number` – tx index within the block.
* `logIndex: number` – log index within the tx.
* `timestamp?: number` – timestamp (when provided by the source).
* `transactionHash?: string` – tx hash.

***

#### CrcV2\_InviteHuman

* `$event: 'CrcV2_InviteHuman'`
* `inviter?: string`
* `invited?: string`

#### CrcV2\_PersonalMint

* `$event: 'CrcV2_PersonalMint'`
* `human?: string`
* `amount?: bigint`
* `startPeriod?: bigint`
* `endPeriod?: bigint`

#### CrcV2\_RegisterGroup

* `$event: 'CrcV2_RegisterGroup'`
* `group?: string`
* `mint?: string`
* `treasury?: string`
* `name?: string`
* `symbol?: string`

#### CrcV2\_RegisterHuman

* `$event: 'CrcV2_RegisterHuman'`
* `avatar?: string`
* `inviter?: string`

#### CrcV2\_RegisterOrganization

* `$event: 'CrcV2_RegisterOrganization'`
* `organization?: string`
* `name?: string`

#### CrcV2\_Stopped

* `$event: 'CrcV2_Stopped'`
* `avatar?: string`

#### CrcV2\_Trust

* `$event: 'CrcV2_Trust'`
* `truster?: string`
* `trustee?: string`
* `expiryTime?: bigint`

#### CrcV2\_TransferSingle

* `$event: 'CrcV2_TransferSingle'`
* `operator?: string`
* `from?: string`
* `to?: string`
* `id?: bigint`
* `value?: bigint`

#### CrcV2\_URI

* `$event: 'CrcV2_URI'`
* `value?: string`
* `id?: bigint`

#### CrcV2\_ApprovalForAll

* `$event: 'CrcV2_ApprovalForAll'`
* `account?: string`
* `operator?: string`
* `approved?: boolean`

#### CrcV2\_TransferBatch

* `$event: 'CrcV2_TransferBatch'`
* `batchIndex: number`
* `operator?: string`
* `from?: string`
* `to?: string`
* `id?: bigint`
* `value?: bigint`

#### CrcV2\_RegisterShortName

* `$event: 'CrcV2_RegisterShortName'`
* `avatar?: string`
* `shortName?: bigint`
* `nonce?: bigint`

#### CrcV2\_UpdateMetadataDigest

* `$event: 'CrcV2_UpdateMetadataDigest'`
* `avatar?: string`
* `metadataDigest?: Uint8Array`

#### CrcV2\_CidV0

* `$event: 'CrcV2_CidV0'`
* `avatar?: string`
* `cidV0Digest?: Uint8Array`

#### CrcV2\_StreamCompleted

* `$event: 'CrcV2_StreamCompleted'`
* `operator?: string`
* `from?: string`
* `to?: string`
* `id?: bigint`
* `amount?: bigint`

#### CrcV2\_CreateVault

* `$event: 'CrcV2_CreateVault'`
* `vault?: string`
* `token?: string`
