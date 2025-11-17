# Inviting and accepting human avatars

Circles allows you to join the network as a human with a token ERC 1155 standard. You would have a profile and would require to be invited to join the network and start minting personal CRC token

### **Inviting a human to Circles**

```typescript
// Inviter side: send invite (escrows 100 CRC + trusts the invitee)
const inviter = await sdk.getAvatar('0xInviterAvatar'); // HumanAvatar
await inviter.invite.send('0xInviteeEOAAddress');
```

* Now,  your invitee can accept the invitation / register by:

{% code overflow="wrap" %}
```typescript
//Invitee side: accept Invitation
const inviteeAvatar = await inviteeSdk.register.asHuman('0xInviterAddress', {
  name: 'Alice',
  description: 'New Circles member',
});

// (Optional) If multiple inviters, redeem a specific one
await inviteeAvatar.invite.redeem('0xSpecificInviter');
```
{% endcode %}

### Get an existing avatar

If you have the address of an existing avatar, you can get the avatar details by:

```typescript
const profile = await inviteeAvatar.profile.get();
if (profile) {
  console.log(profile.name, profile.description);
}
```
