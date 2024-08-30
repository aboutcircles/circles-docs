# 🔗 Understanding Circles Profiles

Profiles manage user data within the Circles SDK and are linked with Circles avatars. The profile data is stored on IPFS and contains attributes such as `name`, `description`, `previewImageUrl`, and `imageUrl.`  An avatar may have an existing profile or can be created post sign up.  Once profile is created and stored on IPFS, it is assigned `CID`  which uniquely identifies the specific avatar profile data. The CID of the profile is recorded as metadata using the ERC1155 token standard. This means that each user's token (which could represent ownership or access rights) includes a reference to the CID.

## Implementing Circles Profile with Circles Avatar

### 1. Initialize the Profiles Service

The `Profiles` class is initialized with a `profileServiceUrl`, which is the base URL for the service managing profiles.

```typescript
import { Profiles } from "@circles-sdk/profiles";
const profiles = new Profiles("https://chiado-pathfinder.aboutcircles.com/profiles/");
```

### 2. Prepare `profile` data

```typescript
import {type Profile} from "@circles-sdk/profiles";
```

```typescript
const profile: Profile = {
    name: "User Name",
    description: "User Description",
    previewImageUrl: "https://example.com/preview.png",
    imageUrl: "https://example.com/image.png"
};
```

### 3. Creating profiles via `create` method

Pass the profile data to `create` method. The method sends a `POST` request to the profile service's `pin` endpoint to store the profile on IPFS. If successful, it returns a CID (Content Identifier) which uniquely identifies the profile data stored on IPFS.

```typescript
const cid = await profiles.create(profile);
console.log(`Profile created with CID: ${cid}`);
```

***

## Read and update a Profile

To read and update profiles, you can get an `AvatarInfo` object and then use it's `getProfile()` and `updateProfile()` methods.

```typescript
const sdk = new Sdk(config, signer);

//
// Retrieve the profile for an avatar
//
const avatar = await sdk.getAvatar(walletAddress);
const profile = await avatar?.getProfile();
if (!profile) {
    console.error("Couldn't load the profile of the avatar");
    return undefined;
}

//
// Update the profile of an avatar.
//
profile.name = "Test test test";

const updatedProfileCID = await avatar.updateProfile(profile);
console.log(`The profile was updated. New CID is: {updatedProfileCid}`);

```

