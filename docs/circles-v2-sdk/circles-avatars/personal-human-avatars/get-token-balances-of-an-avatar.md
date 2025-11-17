# Get token balances of an avatar

You can use the `avatar.balances` helpers to read Circles balances with automatic v1/v2 handling. These methods rely on RPC only and work as soon as the SDK instance is initialized with a runner.

### Get Total Balance

```ts
const totalBalance = await avatar.balances.getTotal();
console.log(`Total Circles balance: ${totalBalance.toString()}`);
```

* Automatically checks the correct hub version based on `avatar.avatarInfo`.
* Returns the sum of all Circles holdings (unwrapped + wrapped) for the avatar address.

### Get Token Balances

```ts
const tokenBalances = await avatar.balances.getTokenBalances();

tokenBalances.forEach((balance) => {
  console.log(`Token: ${balance.token}, Balance: ${balance.amount}`);
});
```

* Returns an array of `TokenBalanceRow` entries for every token relevant to the avatar in the current context.
