---
icon: user
---

# Personal / Human Avatars

A human avatar v2 is implemented through the `V2Avatar` class which implements the `AvatarInterfaceV2` interface. This represents an upgrade from the v1 implementation with enhanced capabilities.

### Key Characteristics of Human Avatar V2

1. **Registration Process**: Human avatars in v2 are created through an invitation system, where an existing user must invite you to join the network:
2. V2 human avatars have associated profiles stored on IPFS, containing information like name, description, and images:
3. **Personal Token Minting**: Human avatars in v2 can mint their own personal Circles tokens
4. V2 human avatars can establish trust relationships with other avatars
5. **Token Transfers**: V2 human avatars can transfer tokens to other avatars, with support for both direct and transitive transfers

#### Data Structure of v2 human avatar

The data for a human avatar v2 is stored in an AvatarRow structure, which would look something like:

```typescript
{  
  blockNumber: 12345678,  
  logIndex: 42,  
  transactionIndex: 3,  
  timestamp: 1620000000,  
  transactionHash: "0x123abc456def789ghi...",  
  version: 2,  
  type: "CrcV2_RegisterHuman",  
  avatar: "0xabcdef1234567890abcdef1234567890abcdef12",  
  tokenId: "0xabcdef1234567890abcdef1234567890abcdef12",  
  hasV1: false,  
  isHuman: true,  
  cidV0Digest: "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",  
  cidV0: "QmT5NvUtoM5nWFfrQdVrFtvGfKFmG7AHE8P34isapyhCxX"  
}
```

## 1. Creation of Personal/Human Avatars :

Circles v2.0 allows you to join the network as a human with an ERC-1155 standard token. You would have a profile and would require to be invited to join the network and start minting personal CRC tokens.

The V2 Hub contract is the main smart contract that a user would interact. You would need a profile CID as well.

<pre class="language-typescript" data-overflow="wrap"><code class="lang-typescript"><strong>const avatar = await avatar.inviteHuman(inviteeAddress,"Hk.....");        //CID required
</strong>
<strong>const avatar = await sdk.acceptInvitation(inviterAddress,"Qm.....");
</strong>console.log(avatar.avatarInfo);
</code></pre>

Incase, you don't have CID, you can use the `Profile` object and implicitly use the Circles pinning service to pin it:

```typescript
const avatar = await sdk.acceptInvitation(inviterAddress, {
    name: "My profile name"
});
console.log(avatar.avatarInfo);
```

## 2. Getting mintable amount for an avatar

This function will allow you to get maximum amount of CRC tokens that are available to mint at that point of time. Human avatars can mint only upto 24 personal Circles per day.

```typescript
const mintableToken = await avatar.getMintableamount ()
```

This function will allow you to mint your personal CRC tokens

```typescript
const mintTransaction = await sdk.personalMint();
console.log('Transaction successful, receipt:', mintTransaction);
```
