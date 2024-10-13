# Inviting and accepting human avatars

## 1. Creation of personal avatars :&#x20;

Circles v2.0 will allow you to join the network as a human with a token ERC 1155 standard. You would have a profile and would require to be invited to join the network and start minting personal CRC tokens.

The V2 Hub contract is the main smart contract that a user would interact. You would need a profile CID as well.

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript"><strong>const avatar = await avatar.inviteHuman(inviteeAddress,"Hk.....");        //CID required
</strong><strong>
</strong><strong>const avatar = await avatar.acceptInvitation(inviterAddress,"Qm.....");
</strong>console.log(avatar.avatarInfo);
</code></pre>

Incase, you don't have CID, you can use the `Profile` object and implicitly use the Circles pinning service to pin it:

```typescript
const avatar = await avatar.acceptInvitation(inviterAddress, {
    name: "My profile name"
});
console.log(avatar.avatarInfo);
```

### Get an existing avatar

If you have the address of an existing avatar, you can get an instance by calling `sdk.getAvatar(address)`. It returns either an `AvatarInterface` instance or throws an error if the avatar can't be found.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```
