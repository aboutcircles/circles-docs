# Creating Organization Avatars

To create an Organization Avatar in Circles v2, you use the `registerOrganizationV2` method from the SDK. This registers the calling wallet address as an Organization Avatar, provided the address is not already registered as another avatar type (Human, Group, or Organization).

Organization Avatars cannot mint tokens but use profiles for identification and can trust other avatars to receive tokens.

You need to provide the organization's profile data when calling the function.

```typescript
// Import the Profile type if needed
// import type { Profile } from '@circles-sdk/profiles';

// Define the organization's profile data
const orgProfileData: Profile = {
    name: "My Organization Name", // Required
    description: "Official profile for My Organization.", // Optional
    // imageUrl: "...", // Optional
    // previewImageUrl: "..." // Optional
};

try {
    // Assuming 'sdk' is your initialized SDK instance
    // The SDK uses the connected wallet address to register the organization
    const newOrgAvatar = await sdk.registerOrganizationV2(orgProfileData);

    console.log('New Organization Avatar created:', newOrgAvatar);
    // You can access info like newOrgAvatar.avatarInfo.address
} catch (error) {
    console.error('Error registering organization:', error);
}
```

{% hint style="info" %}
Ensure the profile data adheres to the required schema and limits outlined in the [Setting Up Circles Profiles](../circles-profiles.md) section.
{% endhint %}
