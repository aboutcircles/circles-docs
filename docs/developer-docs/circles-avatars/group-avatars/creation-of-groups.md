# Creation of groups

You need to pass the profile and the address of the mint policy contract when calling the function.

```typescript

import { GroupProfile, Profile, Profiles } from '@circles-sdk/profiles';

// When on Gnosis Chain use this ..
// const standardMintPolicy = "0x5Ea08c967C69255d82a4d26e36823a720E7D0317";

// When on Chiado use that ..
// const standardMintPolicy = "0xaD49f877021c73d00bE142b135c9AA67f0D8e9c6";

const mintPolicy = '0xYourMintPolicyContractAddress';
const groupProfile: GroupProfile = {
    name: 'Group Namw',
    symbol: 'Group Token Symbol',
    description: '',
    previewImageUrl: '',
    imageUrl: '',
    
    
};
const GroupAvatar = await circlesSDK?.registerGroupV2(mintPolicy, profile);

// Log the newly created group avatar details
console.log('New Group Avatar:', GroupAvatar);
```

{% hint style="info" %}
There are some limits from the profile service:

\
**Profile Name Length:**

* The maximum allowed length for profile names is 36 characters (`config.maxNameLength`). This is enforced in the `validateProfile` function, where it checks if the name exceeds this limit.



**Profile Description Length:**

* The maximum allowed length for descriptions is 500 characters (`config.descriptionLength`), and the `validateProfile` function checks this.



**Image URL Length:**

* The maximum allowed length for image URLs is 2000 characters (`config.imageUrlLength`), validated in the same function.



**Image Validation:**

* The code limits image size to 150 KB (`config.maxImageSizeKB`), and images must be exactly 256x256 pixels (`config.imageDimension`).
* The supported image formats are PNG, JPEG, and GIF.
{% endhint %}

### Circles Profile specs&#x20;

Incase, you want to check how to manage circles profiles, you can simply checkout this guide :

{% content-ref url="../circles-profiles.md" %}
[circles-profiles.md](../circles-profiles.md)
{% endcontent-ref %}



