# Managing Trust Connections

## 1. Trust an Avatar

This function allows the current avatar instance (`avatar`) to trust another avatar (or multiple avatars, depending on implementation). Trusting an avatar signifies willingness to accept Circles issued by them, enabling transfers from the trusted avatar.

```typescript
const targetAvatarAddress = "0xabc..."; // Address of the avatar to trust
try {
  const trustReceipt = await avatar.trust(targetAvatarAddress);
  console.log("Trust established successfully:", trustReceipt);
} catch (error) {
  console.error("Error establishing trust:", error);
}
```

## 2. Untrust an Avatar

This function revokes trust from another avatar (or multiple avatars). Once trust is revoked, the current avatar will no longer accept Circles issued *directly* by the untrusted avatar via path payments. This does not affect Circles already held.

```typescript
const targetAvatarAddress = "0xabc..."; // Address of the avatar to untrust
try {
  const untrustReceipt = await avatar.untrust(targetAvatarAddress);
  console.log("Trust revoked successfully:", untrustReceipt);
} catch (error) {
  console.error("Error revoking trust:", error);
}
```

## 3. Check if Trusted By Another Avatar

This function checks if the current avatar (`avatar`) is trusted by a specific other avatar. It verifies if the other avatar is willing to accept Circles issued by the current avatar.

```typescript
const isTrusted = await avatar.isTrustedBy("AvatarAddress");
console.log(isTrusted); // true or false
```

## 4. Get Trust Relations

This function retrieves all trust relationships involving the current avatar (`avatar`). It returns an array of trust relations, indicating which avatars are trusted by the current avatar, which avatars trust the current avatar, and the status of the relationship (e.g., unidirectional, mutual).

```typescript
try {
  const trustRelations = await avatar.getTrustRelations();
  trustRelations.forEach(relation => {
    // Example: Log the relationship details
    // The exact structure of 'relation' might vary; consult SDK types.
    // Assuming properties like avatar1, avatar2, relationType exist:
    console.log(`Relation: ${relation.avatar1} ${relation.relationType} ${relation.avatar2}`);
  });
} catch (error) {
  console.error("Error getting trust relations:", error);
}
```
