# Creating Group Avatars

To create a new Group Avatar, you need to provide the group's profile data and the address of its chosen mint policy contract using the `registerGroupV2` function.

```typescript
// Import the GroupProfile type
import type { GroupProfile } from '@circles-sdk/profiles';

// Define the mint policy address.
// You can use the standard policy or a custom one.
// Standard Mint Policy on Gnosis Chain:
const standardMintPolicyGnosis = "0xcCa27c26CF7BAC2a9928f42201d48220F0e3a549";
// Standard Mint Policy on RINGS Sandbox:
// const standardMintPolicyRings = "0x79Cbc9C7077dF161b92a745345A6Ade3fC626A60";

const chosenMintPolicy = standardMintPolicyGnosis; // Or your custom policy address

// Define the group's profile data
const groupProfileData: GroupProfile = {
    name: 'My Awesome Group', // Required
    symbol: 'MAG', // Required: Token symbol for the group currency
    description: 'A group for awesome people.', // Optional
    // previewImageUrl: 'data:image/jpeg;base64,...', // Optional
    // imageUrl: 'https://example.com/group_image.png', // Optional
};

try {
  // Assuming 'sdk' is your initialized SDK instance
  const newGroupAvatar = await sdk.registerGroupV2(chosenMintPolicy, groupProfileData);

  // Log the newly created group avatar details
  console.log('New Group Avatar created:', newGroupAvatar);
  // You can access info like newGroupAvatar.avatarInfo.address
} catch (error) {
  console.error("Error registering group avatar:", error);
}

```

{% hint style="info" %}
**Profile Service Limits**

The profile service enforces certain limits on the profile data:

*   **Profile Name Length:** Max 36 characters.
*   **Profile Description Length:** Max 500 characters.
*   **Image URL Length:** Max 2000 characters.
*   **Preview Image Size:** Max 150 KB.
*   **Preview Image Dimensions:** Exactly 256x256 pixels.
*   **Preview Image Formats:** PNG, JPEG, GIF.
{% endhint %}

### Circles Profile Specifications

In case you want to check how to manage Circles profiles in more detail, check out this guide:

{% content-ref url="../../circles-profiles.md" %}
[circles-profiles.md](../../circles-profiles.md)
{% endcontent-ref %}
