# Mint group tokens

Mint Base Group tokens by sending collateral through the trust graph to the group’s mint handler. The `groupToken` helpers on `HumanAvatar` and `OrganisationAvatar` handle pathfinding and wrapped balances automatically.

### Mint Group Tokens

```ts
const groupAddress = '0xGroupAddress';
const amount = BigInt(100); 

try {
  const receipt = await avatar.groupToken.mint(groupAddress, amount);
  console.log('Minting successful:', receipt.hash);
} catch (error) {
  console.error('Minting failed:', error);
}
```

* Uses the pathfinder to route collateral (including wrapped tokens) to the group’s mint handler.
* It will fail if trust or liquidity is insufficient to deliver the requested amount.

### Check Maximum Mintable (Recommended)

```ts
const maxMintable = await avatar.groupToken.getMaxMintableAmount(groupAddress);
console.log('Maximum mintable amount:', maxMintable.toString());
```

This will compute the largest flow to the group’s mint handler with the current trust relations and balances.
