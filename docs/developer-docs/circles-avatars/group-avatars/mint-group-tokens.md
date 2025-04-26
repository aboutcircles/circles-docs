---
description: >-
  This method allows a member of a group to mint that group's currency by
  providing their personal CRC tokens as collateral.
---

# Minting Group Tokens

A member (Human Avatar) can mint a Group Avatar's currency tokens if they trust the Group Avatar. This process involves the member providing an amount of their own personal CRC tokens as collateral, which are then held in the group's vault. In return, the member receives an equivalent amount of the Group Currency tokens.

The specific function might vary, but conceptually, the member initiates the minting process.

```typescript
// Assuming 'memberAvatar' is an SDK instance representing the Human Avatar who is a member
// Assuming 'groupAddress' is the address of the Group Avatar the member wants to mint tokens for
// Assuming the memberAvatar trusts the groupAddress

const groupAddress = '0xYourGroupAddress'; // The address of the group
const amountToMint = BigInt(100); // Amount of group tokens to mint (and personal CRC to collateralize)

try {
    // The member calls a function like 'mintGroupTokens' (actual name might differ in SDK)
    // This function would transfer 'amountToMint' of the member's personal CRC to the vault
    // and mint 'amountToMint' of the group's currency tokens for the member.
    const mintReceipt = await memberAvatar.mintGroupTokens(groupAddress, amountToMint);

    console.log(`Successfully minted ${amountToMint} tokens for group ${groupAddress}:`, mintReceipt);
} catch (error) {
    console.error('Minting group tokens failed:', error);
}
```
{% hint style="info" %}
The exact method name (`mintGroupTokens` used here is illustrative) and parameters might differ based on the specific SDK version and implementation. Consult the SDK reference for the precise function signature. The core principle is that the member provides personal CRC as collateral to receive group tokens.
{% endhint %}
