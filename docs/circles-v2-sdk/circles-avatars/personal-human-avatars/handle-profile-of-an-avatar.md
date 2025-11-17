---
description: This section is dedicated to handling/updating the profiles of an avatar.
---

# Fetching profile of an human avatar

## Get a profile for the avatar

This function fetches the current profile associated with the avatar. If no profile exists, it will return `undefined`.

```typescript
const profile = await inviteeAvatar.profile.get();
if (profile) {
  console.log(profile.name, profile.description);
}
```

## Update profile of the avatar

This function pins JSON to the Profiles service and updates the metadata digest on-chain. Returns the new CID.

```typescript
import type { Profile } from '@aboutcircles/sdk-types';

const newProfile: Profile = {
  name: 'Avatar Name',
  description: 'Updated description for the avatar.',
  avatarUrl: 'ipfs://QmYourImageCIDHere', // optional
};

try {
  const newCid = await inviteeAvatar.profile.update(newProfile);
  console.log('Profile updated. New CID:', newCid);
} catch (err) {
  console.error('Error updating profile:', err);
}
```

If you already have your CID, you can use the function below to update the on-chain pointer:

```ts
const cid = 'QmYourIPFSCIDHere';

try {
  const receipt = await inviteeAvatar.profile.updateMetadata(cid);
  console.log('Metadata updated, tx:', receipt.hash);
} catch (err) {
  console.error('Error updating metadata:', err);
}
```

