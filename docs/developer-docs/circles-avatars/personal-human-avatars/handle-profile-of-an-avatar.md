---
description: This section covers retrieving and updating avatar profile information.
---

# Handling Avatar Profiles

## 1. Get Avatar Profile

This function fetches the profile data associated with the avatar from IPFS, using the CID stored on-chain. If no profile CID is set or the data cannot be retrieved, it may return `undefined` or throw an error depending on the implementation.

```typescript
try {
  const profile = await avatar.getProfile();
  if (profile) {
    console.log("Avatar Profile:", profile);
  } else {
    console.log("No profile associated with this avatar.");
  }
} catch (error) {
  console.error("Error retrieving profile:", error);
}

```

## 2. Update Profile Metadata (CID)

This function directly updates the avatar's on-chain profile reference to a new IPFS Content Identifier (CID). You must ensure the provided CID points to valid profile data adhering to the schema, as this method does not handle data pinning itself.

```typescript
// IPFS CID for the new metadata
const cid = "QmYourIPFSCIDHere";

try {
  const receipt = await avatar.updateMetadata(cid);
  console.log("Metadata updated successfully:", receipt);
} catch (error) {
  console.error("Error updating metadata:", error);
}
```

## 3. Update Profile Data

This function simplifies updating the avatar's profile. You provide a `Profile` object containing the new data. The SDK handles pinning this data to IPFS via the profile service and then updates the avatar's on-chain reference to the new CID. It returns the new CID.

```typescript
// Import the Profile type if needed
// import type { Profile } from '@circles-sdk/profiles';

const newProfileData: Profile = {
  name: "Updated Avatar Name", // Required
  description: "A new description for the avatar.", // Optional
  imageUrl: "https://example.com/new_image.png", // Optional, URL to full image
  previewImageUrl: "data:image/jpeg;base64,/9j/4AAQSk..." // Optional, base64 data URL for preview (must meet size/dimension requirements)
};

try {
  // The SDK pins the newProfileData to IPFS and updates the avatar's metadata reference
  const newCid = await avatar.updateProfile(newProfileData);
  console.log("Profile updated successfully. New CID:", newCid);
} catch (error) {
  console.error("Error updating profile:", error);
}
```
