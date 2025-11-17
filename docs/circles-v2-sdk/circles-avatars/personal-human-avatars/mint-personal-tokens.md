# Mint personal tokens

Human avatars accrue issuance continuously (capped at roughly **24 CRC per day**) and expose helpers under `avatar.personalToken`:

```ts
const mintable = await avatar.personalToken.getMintableAmount();
console.log(`Mintable CRC: ${mintable.amount}`);

if (mintable.amount > 0n) {
  const receipt = await avatar.personalToken.mint();
  console.log('Mint transaction hash:', receipt.hash);
}
```

* `getMintableAmount()` returns the currently claimable amount plus the accrual window.
* `mint()` calls `hubV2.personalMint()` via the configured runner.

