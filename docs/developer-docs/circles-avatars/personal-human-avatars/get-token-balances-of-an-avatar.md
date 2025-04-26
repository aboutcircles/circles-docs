# Getting Token Balances

## 1. Get Total Personal Balance

This function fetches the total balance of the avatar's *own* personal Circles (CRC) tokens from the appropriate Hub contract (v1 or v2, depending on the avatar's version).

{% hint style="info" %}
You can check the avatar's version using the `avatar.avatarInfo` property.
{% endhint %}

```typescript
// Assuming 'avatar' is an instance representing your human avatar
try {
  const totalBalance = await avatar.getTotalBalance();
  console.log(`Total Personal CRC balance: ${totalBalance}`);
} catch (error) {
  console.error("Error getting total balance:", error);
}
```

## 2. Get All Held Balances

This function retrieves the balances of *all* different tokens held by the avatar instance (`avatar`). This includes not only the avatar's own personal CRC but also the tokens of other avatars that this avatar trusts and currently holds. It returns an array of `TokenBalanceRow` objects (structure may vary; consult SDK types), each detailing the token address and the amount held.

```typescript
// Assuming 'avatar' is an instance representing your human avatar
try {
  const allBalances = await avatar.getBalances();
  if (allBalances.length > 0) {
    allBalances.forEach((balance) => {
      // Assuming 'balance' has properties like 'tokenAddress' and 'amount'
      console.log(`Token Address: ${balance.tokenAddress}, Amount Held: ${balance.amount}`);
    });
  } else {
    console.log("Avatar holds no token balances (including its own).");
  }
} catch (error) {
  console.error("Error getting all balances:", error);
}
```
