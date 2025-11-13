---
icon: address-card
---

# Setting Circles Profiles

Circles avatars are ERC‑1155 tokens whose metadata field points to an IPFS CID. The JSON stored at that CID contains the human-friendly profile (name, description, images, etc.) that explorers and UIs render. `@aboutcircles/sdk` exposes helpers on every avatar instance, while `@aboutcircles/sdk-profiles` provides you a lightweight client for direct pin/get calls.

### Profile Data Model

The profile payload mirrors the `Profile` interface from `@aboutcircles/sdk-types`, and the Circles profile service enforces the same shape when accepting uploads.

| Field             | Type                      | Notes                                                                  | Required |
| ----------------- | ------------------------- | ---------------------------------------------------------------------- | -------- |
| `name`            | `string`                  | Display name for the avatar or group.                                  | ✅        |
| `description`     | `string`                  | Free-form text for bios or summaries.                                  |          |
| `previewImageUrl` | `string (uri)`            | Must be a base64 data URL that matches the preview requirements below. |          |
| `imageUrl`        | `string (uri)`            | Full-size image (external URL or data URL).                            |          |
| `location`        | `string`                  | Human-readable location such as `"Berlin, Germany"`.                   |          |
| `geoLocation`     | `[number, number]`        | Tuple of `[latitude, longitude]`.                                      |          |
| `extensions`      | `Record<string, unknown>` | Add your own structured metadata without waiting for schema changes.   |          |

For groups, `GroupProfile` extends `Profile` with a required `symbol` property so downstream tools can display the token ticker alongside other metadata.

```ts
import type { Profile, GroupProfile } from '@aboutcircles/sdk-types';

const humanProfile: Profile = {
  name: 'John Doe',
  description: 'Web3 Developer',
  imageUrl: 'https://example.com/image.jpg',
  previewImageUrl: 'data:image/jpeg;base64,...',
  location: 'Berlin, Germany',
  geoLocation: [52.52, 13.4050],
  extensions: { twitter: '@john' },
};

const groupProfile: GroupProfile = {
  name: 'My Community Group',
  symbol: 'MCG',//REQUIRED
  description: 'A community group for local initiatives',
  imageUrl: 'https://example.com/group-image.jpg',
};
```

### Creating & Pinning Profiles

The SDK automatically creates a `Profiles` client using the `profileServiceUrl` from `circlesConfig`. You can call it directly when building forms or admin panels:

```ts
import { Sdk } from '@aboutcircles/sdk';
import { circlesConfig } from '@aboutcircles/sdk-core';

const runner = /* your ContractRunner */;
await runner.init();

const sdk = new Sdk(circlesConfig[100], runner);
const profile = {
  name: 'John Doe',
  description: 'Web3 Developer',
  previewImageUrl: 'data:image/jpeg;base64,...',
};

const profileCid = await sdk.profiles.create(profile);
console.log('Pinned profile CID:', profileCid);
```

If you already know the CID (for example, you pinned it off-chain), you can fetch it later via `await sdk.profiles.get(cid)` or through any avatar instance using `await avatar.profile.get()`.

#### Updating On-Chain Metadata

For avatars you control:

1. Pin the JSON: `const cid = await avatar.profile.update(profileData);`
2. The helper converts the CIDv0 into the bytes32 digest (`cidV0ToHex`) and submits `nameRegistry.updateMetadataDigest`.
3. Subsequent reads pull the updated profile, and you can register short names via `avatar.profile.registerShortName(nonce)` if desired.

You can also skip the pinning step when you already have a CID:

```ts
await avatar.profile.updateMetadata('QmExampleCidV0');
```

### Group Profiles and Registration

Group metadata uses the same pinning flow, but the registration step also needs the group symbol and deployment parameters for the BaseGroup factory. The SDK’s `register.asGroup` helper wraps all of that:

```ts
import type { GroupProfile } from '@aboutcircles/sdk-types';

const groupProfile: GroupProfile = {
  name: 'My Community Group',
  symbol: 'MCG',
  description: 'A community group for local initiatives',
  imageUrl: 'https://example.com/group-image.jpg',
};

const owner = runner.address!;
const service = circlesConfig[100].coreMembersGroupDeployer;
const feeCollection = circlesConfig[100].standardTreasury;
const initialConditions: Address[] = [];

const groupAvatar = await sdk.register.asGroup(
  owner,
  service,
  feeCollection,
  initialConditions,
  groupProfile.name,
  groupProfile.symbol,
  groupProfile
);
```

`register.asGroup` pins the `groupProfile`, deploys the BaseGroup via `core.baseGroupFactory`, extracts the new group address from the `BaseGroupCreated` event, and returns a `BaseGroupAvatar` wired to your runner.

### Preview Image Requirements

When you embed an image as `previewImageUrl`, the profile service enforces a few constraints to keep responses lightweight:

* Format: PNG, JPEG, or GIF.
* Dimensions: exactly 256×256 pixels.
* File size: ≤ 150 KB after compression.
* Encoding: Base64-encoded data URL (e.g., `data:image/png;base64,...`).

Here’s a browser-friendly helper that crops and compresses user uploads before pinning:

```ts
const preparePreview = (file: File) =>
  new Promise<string>((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => {
      const img = new Image();
      img.src = reader.result as string;
      img.onload = () => {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        const size = 256;

        if (!ctx) return reject(new Error('Canvas context unavailable'));
        canvas.width = canvas.height = size;
        ctx.drawImage(img, 0, 0, size, size);

        const dataUrl = canvas.toDataURL('image/jpeg', 0.5);
        if (dataUrl.length > 150 * 1024) {
          console.warn('Preview exceeds 150 KB after compression');
        }
        resolve(dataUrl);
      };
      img.onerror = reject;
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
```

Profiles that violate those constraints are rejected by the pinning service, so validate on the client and surface helpful errors to your users.

### Reading Profiles Back

* **Via avatars:** `await avatar.profile.get()` pulls and caches the current metadata, while `avatar.profile.update` handles write flows.
* **Via RPC:** `CirclesRpc.avatar.getAvatarInfo` includes the `cidV0` so you can fetch profile blobs without instantiating an avatar class.
* **Via the profiles client:** `await sdk.profiles.get(cid)` is ideal for batch jobs or server-side verification.

Because the CID is stored on-chain, any tool that knows the avatar address can look up its metadata by combining RPC data with the profile service.
