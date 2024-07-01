---
description: >-
  Avatars allow you manage your trust relations and to interact with other
  avatars.
---

# 📝 Interact with other Avatars

1. **Add Trust :** Trusts another avatar. Trusting an avatar means you're willing to accept Circles that have been issued by this avatar.

```javascript
await avatar.trust("0x123");
```

2. **Remove Trust :** Revokes trust from another avatar. This means you will no longer accept Circles issued by this avatar.

```javascript
await avatar.untrust("0x123");
```

3. **personalMint**: Mints the available Circles for the avatar.

```javascript
await avatar.personalMint();
```

4. **transfer**: Transfers Circles to another avatar.

```javascript
const to = "0x..";
const value = 1;
await avatar.transfer(to, value);
```

