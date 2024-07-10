---
description: An avatar represents a Circles user.
---

# 👾 Using avatars

The SDK is built around the concept of avatars. An avatar is a Circles user and is used to interact with other Avatars aka Circles users.

You can get an avatar in two ways:

* Registering a new wallet at Circles via Sign Up,&#x20;
* Loading an existing avatar by it's address.

### Create a new avatar

{% hint style="info" %}
You will need a new Metamask account with some xDAI to be able to interact with contracts and follow along the steps. Make sure you configured MetaMask with the correct ChainID (Gnosis Chain or Chiado) before you continue.
{% endhint %}

### To register a new Human

You can call `registerHuman()` method to sign the connected MetaMask account up for Circles and get your avatar address.

```javascript
const avatar = await sdk.registerHuman();
console.log(avatar.avatarInfo);
```

There are different types of avatars within Circles. A `human` avatar is the most common and suitable for any person. It differentiates itself from all other avatar types in that it can mint 24 new personal Circles per day.

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
const avatar = await sdk.registerOrganizationV2();
```
{% endtab %}

{% tab title="Group" %}
The backbone of Circles are personal currencies. However, because there are so many of them, it's hard to use the personal currencies with many established protocols.

Any Group that's registered in Circles is also a Token. Group members can mint these tokens in exchange for their personal currencies, effectively making all group member's token fungible.

Groups are only available in v2.

```typescript
const avatar = await sdk.registerGroupV2();
```
{% endtab %}
{% endtabs %}

### Next steps

The next page will show you how to use the avatar instance to query essential data like the avatar's Circles balance, it's trust relations and transaction history. On the page after, we'll use it to interact with other avatars.
