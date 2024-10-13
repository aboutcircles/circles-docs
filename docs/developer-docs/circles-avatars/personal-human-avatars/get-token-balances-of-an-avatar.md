# Get token balances of an avatar

## Get total balance of an avatar

This function fetches the total Circles balance of the avatar. It checks the balance from either the v1 or v2 versions of Circles, depending on which version the avatar is using.&#x20;

{% hint style="info" %}
To determine the version, you can refer to the `avatarInfo` property.
{% endhint %}

```typescript
const totalBalance = await avatar.getTotalBalance();
console.log(`Total Circles balance: ${totalBalance}`);
```



## Get balances for an avatar

This function retrieves the avatar's token balances. Before calling this function, ensure that the system is initialized. It returns a promise that resolves to an array of `TokenBalanceRow` objects, each representing the balances for different tokens associated with the avatar in the current context.

```typescript
const tokenBalances = await avatar.getBalances();
tokenBalances.forEach((balance) => {
  console.log(`Token: ${balance.token}, Balance: ${balance.amount}`);
});

```





