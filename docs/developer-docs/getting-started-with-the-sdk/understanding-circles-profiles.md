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

## Retrieving Circles Profiles using CID

To retrive Circles Profile using CID, you need to extract the CID from the `AvatarInfo`

```typescript
const sdk = new Sdk(chainConfig, signer);

async function getProfileData(walletAddress: string): Promise<Profile | undefined> {

    const avatarInfo = await sdk.getAvatar(walletAddress);
    const cid = avatarInfo?.avatarInfo?.cidV0;

    if (!cid) {
        console.error("No CID found for the avatar");
        return undefined;
    }
    
    const profile = await sdk.profiles.get(cid);

    if (!profile) {
        console.error("Profile not found for the given CID");
        return undefined;
    }

    return profile;
}
```

