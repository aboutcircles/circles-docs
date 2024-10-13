---
description: This section is dedicated to handling the profiles of an avatar
---

# Handle profile of an avatar

## Get a profile for the avatar

This function fetches the current profile associated with the avatar. If no profile exists, it will return `undefined`.

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

## Update metadata of the profile

This function updates the avatar's metadata by uploading a new content identifier (CID) to IPFS. The CID represents the new metadata for the avatar.

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

## Update profile of the avatar

This function updates the avatar’s profile and returns the IPFS CID of the newly updated profile.

```typescript
const newProfile: Profile = {
  name: "Avatar Name",
  description: "Updated description for the avatar.",
  image: "ipfs://QmYourImageCIDHere", // Example IPFS image CID
};

try {
  const newCid = await avatar.updateProfile(newProfile);
  console.log("Profile updated successfully. New CID:", newCid);
} catch (error) {
  console.error("Error updating profile:", error);
}
```
