# Managing group invites

In order to invite group members to your group, you would require to trust the members. Trusting them would simply mean that you are inviting them to join group and would be accepting their personal token as collateral. Now, once invited, a group member will require to trust the group avatar address so that they can mint the group tokens.

### Invitation to the group

```typescript
const trustReceipt = await groupAvatar.trust("AvatarAddress");
console.log(receipt);
```

### Removal of the member from the group

```typescript
const trustReceipt = await groupAvatar.untrust("AvatarAddress");
console.log(receipt);
```

