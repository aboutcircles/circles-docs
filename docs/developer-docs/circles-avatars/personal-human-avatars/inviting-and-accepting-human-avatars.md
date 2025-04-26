# Inviting and Accepting Human Avatars

Joining the Circles network as a human avatar requires an invitation from an existing member. This section covers both inviting a new user and accepting an invitation.

## 1. Inviting a Human Avatar

An existing human avatar can invite a new user (represented by their wallet address) to join the network.

```typescript
// Assuming 'existingAvatar' is an SDK instance representing the inviter's avatar
const inviteeAddress = "0x123..."; // Address of the person being invited

try {
  // The inviter calls 'inviteHuman' with the invitee's address
  const inviteTx = await existingAvatar.inviteHuman(inviteeAddress);
  console.log("Invitation sent successfully:", inviteTx);
} catch (error) {
  console.error("Error sending invitation:", error);
}
```

## 2. Accepting an Invitation

The new user (invitee) uses their SDK instance to accept the invitation, providing the inviter's address and their own profile information (either as a pre-pinned CID or as profile data).

```typescript
// Assuming 'sdk' is the SDK instance for the new user (invitee)
const inviterAddress = "0xabc..."; // Address of the person who sent the invitation
const profileCid = "Qm....."; // The invitee's profile CID from IPFS

try {
  // The invitee calls 'acceptInvitation'
  const newAvatar = await sdk.acceptInvitation(inviterAddress, profileCid);
  console.log("Invitation accepted! New avatar created:", newAvatar.avatarInfo);
} catch (error) {
  console.error("Error accepting invitation:", error);
}
```

Alternatively, if the invitee doesn't have a pre-pinned profile CID, they can provide the profile data directly:

```typescript
// Assuming 'sdk' is the SDK instance for the new user (invitee)
const inviterAddress = "0xabc..."; // Address of the person who sent the invitation

try {
  const newAvatar = await sdk.acceptInvitation(inviterAddress, {
    name: "Invitee's Name" // Minimum required profile data
    // description: "...", imageUrl: "...", previewImageUrl: "..."
  });
  console.log("Invitation accepted! New avatar created:", newAvatar.avatarInfo);
} catch (error) {
  console.error("Error accepting invitation:", error);
}
```

## 3. Getting an Existing Avatar

If you know the address of an existing avatar, you can get an SDK instance representing that avatar using `sdk.getAvatar(address)`. This returns an `AvatarInterface` instance or throws an error if the avatar is not found.

```typescript
// Assuming 'sdk' is an initialized SDK instance
const existingAvatarAddress = "0xdef...";

try {
  const avatar = await sdk.getAvatar(existingAvatarAddress);
  console.log("Found avatar:", avatar.avatarInfo);
  // Now you can interact with this avatar instance, e.g., avatar.getProfile(), avatar.trust(...)
} catch (error) {
  console.error(`Error getting avatar ${existingAvatarAddress}:`, error);
}
```
