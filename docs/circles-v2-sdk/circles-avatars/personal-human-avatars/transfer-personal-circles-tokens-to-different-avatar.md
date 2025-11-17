# Transfer personal Circles tokens to different avatar

You can use `avatar.transfer` utilities to reason about and send Circles with or without pathfinding.&#x20;

### Get Maximum Transferable Amount

Pathfinder-based max flow from your avatar to a recipient:

```ts
const maxTransferable = await avatar.transfer.getMaxAmount('0xRecipient');
console.log(`Maximum transferable amount: ${maxTransferable.toString()}`);
```

* Computes the largest flow permitted by your trust graph and current balances.

### Transfer CRC with Pathfinding

```ts
const amount = BigInt(10e18); // 10 CRC
const receipt = await avatar.transfer.advanced('0xRecipient', amount);
console.log(`Transfer successful! Tx: ${receipt.hash}`);
```

* Selects routes through trusted avatars and wrapped tokens automatically.
* Maximum transferable may be lower than your raw balance if trust is missing or tokens are locked in wrappers; check `getMaxAmount` first.

### Direct Token Transfer (Specific Token)

```ts
// Send your personal CRC directly (no pathfinding)
await avatar.transfer.direct('0xRecipient', BigInt(5e18));

// Send a specific wrapped token directly
await avatar.transfer.direct('0xRecipient', BigInt(5e18), '0xWrappedToken');
```

* Bypasses pathfinding; only works if you already hold the target token (personal ERC1155 or wrapped ERC20).
* Provide a token address to send wrapped balances or omit to use your personal token.
