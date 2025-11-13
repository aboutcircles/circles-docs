# Mint personal tokens

## Get mintable amount for an avatar

This function will allow you to get maximum amount of CRC tokens that are available to mint at that point of time. Human avatars can mint only upto 24 personal Circles per day.

```typescript
const mintableToken = await avatar.getMintableamount();
```



## Mint personal tokens :&#x20;

This function will allow you to mint your personal CRC tokens

```typescript
const mintTransaction = await avatar.personalMint();
console.log('Transaction successful, receipt:', mintTransaction);
```
