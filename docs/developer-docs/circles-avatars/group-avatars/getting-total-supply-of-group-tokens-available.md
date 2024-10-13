# Getting total supply of group tokens available

This method retrieves the total amount of Circles associated with the avatar, which can be either Personal Circles or Group Circles, depending on the context in which it is called. This is useful for understanding the overall balance of Circles held by the avatar.

```typescript
getTotalSupply: () => Promise<bigint>;

// Example of calling the getTotalSupply method
try {
    const totalSupply = await circles.getTotalSupply();
    console.log('Total Circles Supply:', totalSupply.toString());
} catch (error) {
    console.error('Error fetching total supply:', error);
}
```

