---
description: An avatar represents a Circles user.
icon: user-ninja
---

# Using avatars

The SDK is built around the concept of avatars. An avatar is a Circles user and is used to interact with other Avatars.&#x20;

* For new Circles users, you would require them to Sign up.
* For existing Circles users, you can simply get exisiting avatars by address.

### Creating a new avatar

{% hint style="info" %}
You will need a Metamask account with some xDAI to be able to interact with contracts and follow along the steps. Make sure you configured MetaMask with the correct ChainID (Gnosis Chain or Chiado) before you continue.
{% endhint %}

### To register a new Human

You can call `registerHuman()` method to sign the connected MetaMask account up for Circles and get your avatar address.&#x20;

There are different types of avatars within Circles. A `human` avatar is the most common and suitable for any person. It differentiates itself from all other avatar types in that it can mint 24 new personal Circles per day.

{% tabs %}
{% tab title="Circles V2" %}
In v2, all avatars need a profile CID. You can either bring you own (CID) and need to take care that it's available ...

{% code overflow="wrap" %}
```typescript
const avatar = await sdk.acceptInvitation(inviterAddress,"Qm.....");
console.log(avatar.avatarInfo);
```
{% endcode %}

... or fill in the `Profile` object and implicitly use the Circles pinning service to pin it:

```typescript
const avatar = await sdk.acceptInvitation(inviterAddress, {
    name: "My profile name"
});
console.log(avatar.avatarInfo);
```
{% endtab %}

{% tab title="Circles V1" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>const avatar = await sdk.registerHuman();
</strong>console.log(avatar.avatarInfo);
</code></pre>
{% endtab %}
{% endtabs %}

### Get an existing avatar

If you have the address of an existing avatar, you can get an instance by calling `sdk.getAvatar(address)`. It returns either an `AvatarInterface` instance or throws an error if the avatar can't be found.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```

{% hint style="info" %}
You can supply any avatar address to the function, even if you don't have the key for it. However, if you just want to query the data with the `@circles-sdk/data` package you can refer to [this part of documentation](https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/query-data).
{% endhint %}

### Different Avatar types

Apart from `Human` Avatar you can also create `Group` and `Orangization` Avatar types. The technical details of same will be mentioned in later sections of docs. But here's how you can sign up as Group or Organization Avatar.

{% hint style="info" %}
One account can only sign up as one avatar. E.g. if an account is already signed up as human then it can't sign-up as organization.
{% endhint %}

{% tabs %}
{% tab title="Organization" %}
Organizations can be used e.g. by shop owners to receive payments in Circles. They are available in Circles v1 and v2. Organizations can't mint new Circles.

```typescript
// v1:
const avatar = await sdk.registerOrganization();
```

```typescript
// v2:
const avatar = await sdk.registerOrganizationV2({
  name: "Test shop"
});
```
{% endtab %}

{% tab title="Group" %}
The backbone of Circles are personal currencies. However, because there are so many of them, it's hard to use the personal currencies with many established protocols.

Any Group that's registered in Circles is also a Token. Group members can mint these tokens in exchange for their personal currencies, effectively making all group member's token fungible.

Groups are only available in v2.

```typescript
import { GroupProfile, Profile, Profiles } from '@circles-sdk/profiles';

// When on Gnosis Chain use this ..
// const standardMintPolicy = "0x5Ea08c967C69255d82a4d26e36823a720E7D0317";

// When on Chiado use that ..
// const standardMintPolicy = "0xaD49f877021c73d00bE142b135c9AA67f0D8e9c6";

const groupProfile: GroupProfile = {
    name: "Test group",
    symbol: "TGRP"
};
const avatar = await sdk.registerGroupV2(standardMintPolicy, groupProfile);
```
{% endtab %}
{% endtabs %}

## Read and update a Profile

To read and update profiles, you can get an `Avatar` object and then use it's `getProfile()` and `updateProfile()` methods. These methods work for all avatar types.

```typescript
// Retrieve the profile for an avatar

const avatar = await sdk.getAvatar(walletAddress);
const profile = await avatar?.getProfile();
if (!profile) {
    console.error("Couldn't load the profile of the avatar");
    return undefined;
}

// Update the profile of an avatar.
profile.name = "Test test test";

const updatedProfileCID = await avatar.updateProfile(profile);
console.log(`The profile was updated. New CID is: {updatedProfileCid}`);

```

If you want to store a profile picture, make sure it adheres to the [circles-profiles.md](../circles-profiles.md "mention") specification.

### Next steps

The next page will show you how to use the avatar instance to query essential data like the avatar's Circles balance, it's trust relations and transaction history. On the page after, we'll use it to interact with other avatars.

***

### Circles SDK Avatar Implementation

{% tabs %}
{% tab title="React" %}
For complete implementation for CirclesSDK Avatar check out the circles implementation : [https://github.com/vanshika-srivastava/circles-vite-app/blob/circles-sdk-v0.9.0/src/circles-components/circlesonboarding.jsx](https://github.com/vanshika-srivastava/circles-vite-app/blob/circles-sdk-v0.9.0/src/circles-components/circlesonboarding.jsx)
{% endtab %}

{% tab title="Svelte" %}
For a complete example that registers a human or organization account, check out the [Circles SDK Svelte Examples](https://github.com/aboutcircles/circles-sdk-svelte-examples) on GitHub. Specifically the [Using avatars](https://github.com/aboutcircles/circles-sdk-svelte-examples/blob/master/src/routes/using-avatars/%2Bpage.svelte) route.
{% endtab %}
{% endtabs %}
