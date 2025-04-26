---
icon: user
---

# Personal (Human) Avatars

## 1. Creating Personal (Human) Avatars

Circles v2.0 allows you to join the network as a human avatar using an ERC-1155 standard token. You will have a profile and require an invitation to join the network and start minting personal CRC tokens.

The V2 Hub Contract is the main smart contract a user interacts with for these operations. A profile CID (Content Identifier for IPFS) is also required when accepting an invitation.

To accept an invitation and create your human avatar:

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript">// Use the SDK's acceptInvitation method, providing the inviter's address and your profile CID.
<strong>const avatar = await sdk.acceptInvitation(inviterAddress, "Qm....."); // Replace "Qm..." with actual profile CID
</strong>
// The 'avatar' object now represents your newly created human avatar.
console.log(avatar.avatarInfo);
</code></pre>

In case you don't have a pre-pinned profile CID, you can provide a `Profile` object directly. The SDK will implicitly use the Circles profile service to pin the profile data to IPFS and get the CID:

```typescript
// Provide profile data directly; the SDK handles pinning to IPFS.
const avatar = await sdk.acceptInvitation(inviterAddress, {
    name: "My profile name" // 'name' is the minimum required field
    // description: "Optional description",
    // imageUrl: "Optional image URL",
    // previewImageUrl: "Optional base64 preview image data URL"
});
console.log(avatar.avatarInfo);
```

## 2. Getting Mintable Amount

This function allows you to check the maximum amount of personal CRC tokens available for your avatar to mint at the current time. Human avatars can mint up to 24 personal Circles per day, minus any demurrage effects if applicable based on the minting period.

```typescript
// Assuming 'avatar' is an instance representing your human avatar
const mintableTokens = await avatar.getMintableAmount();
console.log(`Available to mint: ${mintableTokens}`);
```

## 3. Minting Personal Tokens

This function allows your avatar to mint its available personal CRC tokens.

```typescript
// Assuming 'sdk' is your initialized SDK instance
const mintTransaction = await sdk.personalMint();
console.log('Minting transaction successful, receipt:', mintTransaction);
```
