# Getting Total Supply of Group Tokens

When called on a Group Avatar instance, this method retrieves the total current supply of that specific *group's* currency tokens. This represents the total amount of this group's currency that has been minted (by members collateralizing their personal CRC).

```typescript
// Assuming 'groupAvatar' is an SDK instance representing the specific Group Avatar
try {
    const totalSupply = await groupAvatar.getTotalSupply();
    console.log(`Total supply for group ${groupAvatar.avatarInfo.address}:`, totalSupply.toString());
} catch (error) {
    console.error('Error fetching total supply for group:', error);
}
```
