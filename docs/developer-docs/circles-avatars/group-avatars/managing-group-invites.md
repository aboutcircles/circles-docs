# Managing Group Membership (Invites)

Membership in a Group Avatar is managed through trust relationships initiated by the Group Avatar itself (typically controlled by its owner/admin).

When a Group Avatar `trust`s a Human Avatar, it effectively invites or grants membership to that human. Conversely, when the Group Avatar `untrust`s a Human Avatar, their membership is revoked.

Note: For a member to *mint* group tokens, the Human Avatar must separately trust the Group Avatar address (allowing them to provide collateral). This section focuses only on the group granting/revoking membership status.

## 1. Invite Member (Grant Membership)

The Group Avatar instance calls `trust` on the target Human Avatar's address.

```typescript
// Assuming 'groupAvatar' is an SDK instance representing the Group Avatar
const memberAddress = "0xabc..."; // Address of the Human Avatar to invite

try {
  const trustReceipt = await groupAvatar.trust(memberAddress);
  console.log(`Successfully invited/granted membership to ${memberAddress}:`, trustReceipt);
} catch (error) {
  console.error(`Error inviting member ${memberAddress}:`, error);
}
```

## 2. Remove Member (Revoke Membership)

The Group Avatar instance calls `untrust` on the target Human Avatar's address.

```typescript
// Assuming 'groupAvatar' is an SDK instance representing the Group Avatar
const memberAddress = "0xabc..."; // Address of the Human Avatar to remove

try {
  const untrustReceipt = await groupAvatar.untrust(memberAddress);
  console.log(`Successfully removed/revoked membership for ${memberAddress}:`, untrustReceipt);
} catch (error) {
  console.error(`Error removing member ${memberAddress}:`, error);
}
```
