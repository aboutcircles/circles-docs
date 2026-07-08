# Getting total supply of group tokens available

Use `balances.getTotalSupply()` on avatars that expose a minted token (currently Base Groups).

#### Get Total Supply for a Base Group

```ts
import { Sdk, BaseGroupAvatar } from '@aboutcircles/sdk';
import { circlesConfig } from '@aboutcircles/sdk-core';

const sdk = new Sdk(circlesConfig[100], runner);
const avatar = await sdk.getAvatar('0xGroupAddress');

if (avatar instanceof BaseGroupAvatar) {
  const totalSupply = await avatar.balances.getTotalSupply();
  console.log('Total group token supply:', totalSupply.toString());
} else {
  console.log('Total supply is only available for BaseGroup avatars.');
}
```

* Returns the ERC‑1155 total supply for the group’s token ID.

{% hint style="info" %}
If you call `getTotalSupply()` on a Human or Organisation avatar you’ll receive an `unsupportedOperation` error; use balance methods (`balances.getTotal`, `balances.getTokenBalances`) instead for those types.
{% endhint %}
