# Transferring Personal Circles Tokens

## 1. Get Maximum Transferable Amount

This function utilizes the Pathfinder service to determine the maximum amount of Circles (CRC) that the current avatar instance (`avatar`) can transfer to a specified recipient avatar (`toAvatarAddress`) through the existing trust network paths. This amount may be less than the avatar's total balance due to the capacity limits of the trust paths between the sender and receiver.

```typescript
// Assuming 'avatar' is an instance representing the sender's human avatar
const recipientAddress = "0xabc..."; // Address of the recipient avatar

try {
  const maxTransferable = await avatar.getMaxTransferableAmount(recipientAddress);
  console.log(`Maximum transferable amount to ${recipientAddress}: ${maxTransferable}`);
} catch (error) {
  console.error("Error getting maximum transferable amount:", error);
}
```

## 2. Transfer CRC Tokens

This function initiates a transfer of CRC tokens from the current avatar instance (`avatar`) to the specified recipient avatar. The transfer relies on finding a valid trust path via the Pathfinder. Ensure the `amountToTransfer` does not exceed the maximum transferable amount determined by `getMaxTransferableAmount`.

```typescript
// Assuming 'avatar' is an instance representing the sender's human avatar
const recipientAddress = "0xabc..."; // Address of the recipient avatar
const amountToTransfer = 10; // Example amount

try {
  // Ensure amountToTransfer <= maxTransferable before calling transfer
  const transferReceipt = await avatar.transfer(recipientAddress, amountToTransfer);
  console.log(`Transfer successful! Transaction receipt:`, transferReceipt);
} catch (error) {
  console.error("Error transferring tokens:", error);
}
```
