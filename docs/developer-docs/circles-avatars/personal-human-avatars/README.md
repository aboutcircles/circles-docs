---
icon: user
---

# Personal / Human Avatars

## 1. Creation of Personal/Human Avatars :

Circles v2.0 allows you to join the network as a human with an ERC-1155 standard token. You would have a profile and would require to be invited to join the network and start minting personal CRC tokens.

The V2 Hub contract is the main smart contract that a user would interact. You would need a profile CID as well.

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript"><strong>const avatar = await avatar.inviteHuman(inviteeAddress,"Hk.....");        //CID required
</strong>
<strong>const avatar = await sdk.acceptInvitation(inviterAddress,"Qm.....");
</strong>console.log(avatar.avatarInfo);
</code></pre>

Incase, you don't have CID, you can use the `Profile` object and implicitly use the Circles pinning service to pin it:

```typescript
const avatar = await sdk.acceptInvitation(inviterAddress, {
    name: "My profile name"
});
console.log(avatar.avatarInfo);
```

## 2. Getting mintable amount for an avatar

This function will allow you to get maximum amount of CRC tokens that are available to mint at that point of time. Human avatars can mint only upto 24 personal Circles per day.

```typescript
const mintableToken = await avatar.getMintableamount ()
```

## 3. Minting personal tokens :

This function will allow you to mint your personal CRC tokens

```typescript
const mintTransaction = await sdk.personalMint();
console.log('Transaction successful, receipt:', mintTransaction);
```
