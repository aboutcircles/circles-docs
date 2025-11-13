---
icon: address-card
---

# Setting Circles Profiles

**Circles** is built around the **ERC-1155 token standard**, which allows tokens to include metadata. Since Circles is all about personal currency, it makes sense to use this metadata to define a profile.

The profile data is stored on **IPFS**, and the Circles avatar references the **CIDv0** of the profile on-chain. Using the profile interface from the **Circles SDK**, you can create user profiles for all avatars.

**Required Field:**

* `name`: A string representing the user's name

**Optional Fields:**

* `description`: A string for additional user information
* `previewImageUrl`: A string URL for a preview/thumbnail image
* `imageUrl`: A string URL for a full-size profile image
* `location`: A string representing the user's location (e.g., "Berlin, Germany")
* `geoLocation`: A tuple of two numbers representing coordinates `[latitude, longitude]`
* `extensions`: A flexible `Record` type for storing additional custom data

### Example for creating user profile:



1. Let's create a test profile object.

```typescript
const profile = {  
  name: 'John Doe',  
  description: 'Web3 Developer',  
  imageUrl: 'https://example.com/image.jpg',  
  previewImageUrl: 'https://example.com/preview.jpg',  
  location: 'Berlin, Germany',  
  geoLocation: [52.5200, 13.4050]  
};  
```

2. You should already have circles SDK initialized to create profile.&#x20;

```typescript
//Accessing profiles through the SDK  
const profileCID = await sdk.profiles.create(profile);
```

### Creating profile for your groups

**The Group profile** is used for groups created using Circles, each of which has its own token symbol. The `GroupProfile` inherits all fields from the base `Profile`—such as `name`, `description`, images, `location`, etc.—and adds a required `symbol` field representing the group's token symbol.

```typescript
import { Sdk } from '@circles-sdk/sdk';  
import { GroupProfile } from '@circles-sdk/profiles';  
  
// Initialize the SDK - if not already
const sdk = new Sdk(contractRunner, config);  
  
// Create a group profile  
const groupProfile: GroupProfile = {  
  name: 'My Community Group',  
  symbol: 'MCG',  
  description: 'A community group for local initiatives',  
  imageUrl: 'https://example.com/group-image.jpg',  
  location: 'Berlin, Germany'  
};  
  
// Register the group with a mint policy address  
const mintPolicyAddress = '0x1234...'; // The address of the minting policy to use  
const groupAvatar = await sdk.registerGroupV2(mintPolicyAddress, groupProfile);
```

### Profile schema

```json
{  
  "$schema": "http://json-schema.org/draft-07/schema#",  
  "title": "Circles Profile Schema",  
  "type": "object",  
  "required": ["name"],  
  "properties": {  
    "name": {  
      "type": "string",  
      "description": "The user's name (required)"  
    },  
    "description": {  
      "type": "string",  
      "description": "Additional information about the user"  
    },  
    "previewImageUrl": {  
      "type": "string",  
      "format": "uri",  
      "description": "URL for a preview/thumbnail image"  
    },  
    "imageUrl": {  
      "type": "string",  
      "format": "uri",  
      "description": "URL for a full-size profile image"  
    },  
    "location": {  
      "type": "string",  
      "description": "Text representation of the user's location (e.g., 'Berlin, Germany')"  
    },  
    "geoLocation": {  
      "type": "array",  
      "minItems": 2,  
      "maxItems": 2,  
      "items": {  
        "type": "number"  
      },  
      "description": "Coordinates as [latitude, longitude]"  
    },  
    "extensions": {  
      "type": "object",  
      "description": "Additional custom data as key-value pairs"  
    }  
  }  
}
```

### Profile picture

You can include a profile picture in the profile document (the `previewImageUrl`). If you choose to do so, make sure the picture you are using has the following properties:

1. **Format**: The image must be in PNG, JPEG, or GIF format.
2. **Dimensions**: The image must be exactly 256x256 pixels.
3. **File size**: The image must not exceed 150KB.
4. **Encoding**: The image must be base64 encoded and included as a data URL in the `previewImageUrl` field.

These requirements are enforced by the server to ensure consistency and performance across the platform.

```typescript
  const img = new Image();
        img.src = reader.result as string;
        img.onload = () => {
          const canvas = document.createElement('canvas');
          const ctx = canvas.getContext('2d');
          const cropWidth = 256;
          const cropHeight = 256;

          if (ctx) {
            canvas.width = cropWidth;
            canvas.height = cropHeight;

            ctx.drawImage(img, 0, 0, cropWidth, cropHeight);

            const imageDataUrl = canvas.toDataURL('image/jpeg', 0.5);

            if (imageDataUrl.length > 150 * 1024) {
              console.warn('Image size exceeds 150 KB after compression');
            }

```

&#x20;[Here](https://github.com/aboutcircles/5ecret-garden/blob/06da3e5d472b487fa8e1bc726561eddbc97fdd30/circles-app/src/lib/components/ImageUpload.svelte#L33) is a code example (Browser; TypeScript) that shows how you can prepare the previewImageUrl.

{% hint style="warning" %}
Profiles that don't adhere to the spec aren't considered and won't be served by Circles' profile service.
{% endhint %}

