# Manage trust connections

## Trust an avatar

This function allows an avatar to trust another avatar or multiple avatars. Trusting an avatar means you are willing to accept Circles that have been issued by them. Once trusted, Circles transfers from the trusted avatar are allowed.

```typescript
const trustReceipt = await avatar.trust("AvatarAddress");
console.log(receipt);
```

## Untrust an avatar

This function revokes trust from another avatar or multiple avatars. Once trust is revoked, the avatar will no longer accept Circles issued by the untrusted avatar. However, this will not impact Circles that were already received from the untrusted avatar.

```typescript
const untrustReceipt = await avatar.untrust("AvatarAddress");
console.log(receipt);
```

## Check if avatar trusts another Avatar

This function checks if the current avatar is trusted by another avatar. It can be used to verify whether another avatar is willing to accept Circles issued by the current avatar.

```typescript
const isTrusted = await avatar.isTrustedBy("AvatarAddress");
console.log(isTrusted); // true or false
```

This function retrieves all trust relationships of the current avatar. It returns an array of trust relations indicating which avatars are trusted, which avatars trust the current avatar, and which relationships are mutual.

```typescript
const trustRelations = await avatar.getTrustRelations();
trustRelations.forEach
    (relation => { 
        console.log(${relation.avatar1} ${relation.relation} ${relation.avatar2}); });
```

