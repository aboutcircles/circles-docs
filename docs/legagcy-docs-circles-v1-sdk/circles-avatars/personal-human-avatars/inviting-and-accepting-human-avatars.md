# Inviting and accepting human avatars

## 1. Creation of Personal/Human Avatars :

Circles v2.0 will allow you to join the network as a human with a token ERC 1155 standard. You would have a profile and would require to be invited to join the network and start minting personal CRC tokens.

The V2 Hub contract is the main smart contract that a user would interact with. When you initialize an Avatar, it determines the appropriate implementation based on the avatar's version and type.



* **Inviting a human to Circles (V2 only)**:

```typescript
await avatar.inviteHuman(newUserAddress);
```

* Now, once your invited person has created it's profile, we will pass this along with inviter address

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript"><strong>const avatar = await sdk.acceptInvitation(inviterAddress, profile);
</strong>console.log(avatar.avatarInfo);
</code></pre>

### Get an existing avatar

If you have the address of an existing avatar, you can get an instance by calling `sdk.getAvatar(address)`. It returns either an `AvatarInterface` instance or throws an error if the avatar can't be found.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```
