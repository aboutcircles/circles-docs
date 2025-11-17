---
icon: user
---

# Personal / Human Avatars

Circles SDK  exposes human avatars through the `HumanAvatar` class, which implements the `AvatarInterfaceV2` contract. Each instance wraps the Circles V2 hub contracts, profile service, and pathfinding helpers so apps can manage invitations, minting, trust, and transfers with minimal boilerplate.

### Key Characteristics

* **Invitation-based onboarding** – every human avatar must be invited by an existing avatar that escrows 100 CRC via `avatar.invite.send`.
* **Profile-backed identities** – v2 humans always reference an IPFS CID (pinned through the Profiles service) that stores metadata such as name, description, and media URLs.
* **Personal token minting** – humans accrue up to 24 CRC per day and can mint accumulated issuance via `avatar.personalToken.mint`.
* **Trust graph participation** – `avatar.trust.add/remove/isTrusting` manages bilateral trust needed for pathfinding and transfers.
* **Transfer orchestration** – `avatar.transfer.direct` and `avatar.transfer.advanced` support both direct CRC transfers and multi-hop, wrapped-balance flows.

### AvatarRow Data

Human avatars retrieved from `Sdk#getAvatar` or `CirclesRpc.avatar.getAvatarInfo` surface an `AvatarRow` payload. V2 humans are tagged with `CrcV2_RegisterHuman` events and look like:

```json
{
  "blockNumber": 12345678,
  "logIndex": 42,
  "transactionIndex": 3,
  "timestamp": 1620000000,
  "transactionHash": "0x123abc456def789ghi...",
  "version": 2,
  "type": "CrcV2_RegisterHuman",
  "avatar": "0xabcdef1234567890abcdef1234567890abcdef12",
  "tokenId": "0xabcdef1234567890abcdef1234567890abcdef12",
  "hasV1": false,
  "isHuman": true,
  "cidV0Digest": "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
  "cidV0": "QmT5NvUtoM5nWFfrQdVrFtvGfKFmG7AHE8P34isapyhCxX"
}
```

* `avatar` / `tokenId` – human avatars use their address as the ERC‑1155 token id.
* `cidV0` – IPFS CID pointing to the profile JSON; `cidV0Digest` is the on-chain metadata digest.
* `hasV1` – indicates whether the address has a legacy v1 identity that can still be migrated.
* `timestamp`, `blockNumber`, `logIndex` – useful for freshness checks or historical queries.

### Creating V2 Human Avatars

1. **Invite the new human** – the inviter uses an existing `HumanAvatar` (fetched via `Sdk#getAvatar`) and escrows 100 CRC:

```ts
const sdk = new Sdk(circlesConfig[100], runner);
const inviter = await sdk.getAvatar('0xInviter...');
await inviter.invite.send('0xInvitee...');
```

2. **Register / accept invitation** – the invitee calls `sdk.register.asHuman`. Pass either an existing CID or a `Profile` object; the SDK auto-pins profile objects to the Circles profile service before registering with the hub.

```ts
const sdk = new Sdk(circlesConfig[100], inviteeRunner);

// Accept using an existing CID
const avatarFromCid = await sdk.register.asHuman('0xInviter...', 'QmCID...');
console.log(avatarFromCid.avatarInfo);

// Accept using an inline profile (CID is created under the hood)
const avatarFromProfile = await sdk.register.asHuman('0xInviter...', {
  name: 'My profile name',
  description: 'Coop member',
  avatarUrl: 'https://example.com/me.png'
});
console.log(avatarFromProfile.avatarInfo);
```

### Working With Profiles

* `avatar.profile.get()` – fetches and caches the IPFS JSON referenced by `cidV0`.
* `avatar.profile.update(profile)` – pins new metadata and updates the NameRegistry digest; useful for avatar name/description/image changes.
* `sdk.register.asHuman(inviter, profileOrCid)` – automatically uses the Profiles service when a plain object is supplied, so invitees do not need prior CID knowledge.

###

### Trust & Transfers

* Establish trust through `avatar.trust.add('0xFriend...', expiry?)`, remove trust via `avatar.trust.remove`, and inspect relationships using `avatar.trust.getAll()` or `avatar.trust.isTrusting`.
* Transfer CRC with `avatar.transfer.direct('0xRecipient...', 25n * 10n ** 18n)` for simple paths, or `avatar.transfer.advanced` to opt into wrapped balances, token filters, or encoded metadata.
* Replenish personal (unwrapped) CRC by converting wrapped holdings with `avatar.balances.replenish()`; the SDK uses the pathfinder to determine the optimal flow back to the avatar’s personal token.
