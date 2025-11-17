# Managing group invites

Base groups “invite” members by trusting their personal avatars; members must also trust the group avatar to mint group tokens as collateral.

### Invite a Member (Trust Them)

```ts
const trustReceipt = await groupAvatar.trust.add('0xMemberAvatar');
console.log('Invitation sent (trust added):', trustReceipt.hash);
```

* Trusting a member signals that the group will accept their personal token as collateral.
* Safe runners can batch invites: `groupAvatar.trust.add(['0xA', '0xB']);`.

### Member Accepts (Trust the Group)

```ts
const memberAvatar = await sdk.getAvatar('0xMemberAvatar');
await memberAvatar.trust.add(groupAvatar.address);
```

* Members must trust the group avatar before they can mint group tokens.

### Revoke a Member (Untrust Them)

```ts
const revokeReceipt = await groupAvatar.trust.remove('0xMemberAvatar');
console.log('Revoked member (trust removed):', revokeReceipt.hash);
```

* Batching works the same way: `groupAvatar.trust.remove(['0xA', '0xB']);`.
