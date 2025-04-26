# Minting Personal Tokens

## 1. Get Mintable Amount

This function allows you to check the maximum amount of personal CRC tokens available for the avatar instance (`avatar`) to mint at the current time. Human avatars can mint up to 24 personal Circles per day, adjusted for demurrage based on the minting period.

```typescript
// Assuming 'avatar' is an instance representing your human avatar
try {
  const mintableTokens = await avatar.getMintableAmount();
  console.log(`Available to mint: ${mintableTokens}`);
} catch (error) {
  console.error("Error getting mintable amount:", error);
}
```

## 2. Mint Personal Tokens

This function allows the avatar instance (`avatar`) to mint its available personal CRC tokens.

```typescript
// Assuming 'avatar' is an instance representing your human avatar
try {
  const mintReceipt = await avatar.personalMint();
  console.log('Minting transaction successful, receipt:', mintReceipt);
} catch (error) {
  console.error("Error minting personal tokens:", error);
}
```
