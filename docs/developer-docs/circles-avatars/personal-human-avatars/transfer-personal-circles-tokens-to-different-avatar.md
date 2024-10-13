# Transfer personal Circles tokens to different avatar

## Get Maximum amount of transferrable token

Utilizes the pathfinder to find the maximum Circles amount that can be transferred from this Avatar to the specified avatar. The address of the avatar passed would be the one to which the Circles will be transferred.

```typescript
const maxTransferable = await avatar.getMaxTransferableAmount(toAvatarAddress)
console.log(`Maximum transferable amount: ${maxTransferable}`);
```



## Transfer CRC tokens

This function will allow you to transfer CRC tokens to the avatars with a valid trust path. The maximum transferable amount can be lower than the avatar's balance depending on its trust relations and token holdings.

```typescript
const transferReceipt = await avatar.transfer(recipientAddress, amountToTransfer);
console.log(`Transfer successful! Transaction receipt: ${transferReceipt}`);
```

