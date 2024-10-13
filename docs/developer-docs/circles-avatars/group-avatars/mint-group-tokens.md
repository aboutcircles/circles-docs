---
description: >-
  This method allows a group to mint new Group Circles using trusted collateral
  tokens. The group specifies which collateral to use and the corresponding
  amounts.
---

# Mint group tokens

{% code overflow="wrap" %}
```typescript
groupMint: (
    group: string, 
    collateral: string[], 
    amounts: bigint[], 
    // data: Uint8Array                                         optional
) => Promise<ContractTransactionReceipt>;


const groupAddress = '0xYourGroupAddress'; // The address of the group
const collateralTokens = ['0xToken1', '0xToken2']; 
// Addresses of the collateral tokens
const collateralAmounts = [BigInt(1000), BigInt(2000)]; 
// Corresponding amounts for each token

try {
    const receipt = await circles.groupMint(groupAddress, collateralTokens, collateralAmounts);

    console.log('Minting successful:', receipt);
} catch (error) {
    console.error('Minting failed:', error);
```
{% endcode %}

