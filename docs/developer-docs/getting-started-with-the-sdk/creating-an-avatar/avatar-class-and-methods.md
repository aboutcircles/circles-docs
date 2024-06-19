---
description: The Avatar class provides the following methods from the Circles SDK.
---

# 📝 Avatar class and methods

1. **Add Trust :** Trusts another avatar. Trusting an avatar means you're willing to accept Circles that have been issued by this avatar.

```javascript
async function addTrust() {
      await avatar.trust("0x123");                        //enter the address of avatar
    }
```

2. **Remove Trust :** Revokes trust from another avatar. This means you will no longer accept Circles issued by this avatar.

```javascript
async function removeTrust() {
      await avatar.untrust("0x123");                     //enter the address of avatar
    }
```

3. **getTrustRelations**: Gets the current incoming and outgoing trust relations of the avatar.

```javascript
async function getTrustRelations() {
    await avatar.getTrustRelations("0x123");
}

```

4. **personalMint**: Mints the available Circles for the avatar.

```javascript
async function personalMint() {
    await avatar.getMintableAmount("0x123");
}
```

5. **getMintableAmount**: Gets the amount available to mint via `personalMint`.

```javascript
async function getMintable() {
    await avatar.getMintableAmount("0x123");
}
```

6. **transfer**: Transfers Circles to another avatar.

```javascript
async function transfer() {
    await avatar.transfer("0x123");
}
```

7. **getTotalBalance**: Gets the total balance of the avatar.

```javascript
async function getBalance() {
    await avatar.getTotalBalance("0x123");
}
```

8. **getTransactionHistory**: Gets the transaction history of the avatar.

```javascript
async function getTxnHistory() {
    await avatar.getTransactionHistory("0x123");
}
```
