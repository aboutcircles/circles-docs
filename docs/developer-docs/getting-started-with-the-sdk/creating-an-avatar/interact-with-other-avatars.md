---
description: >-
  Avatars allow you to manage your trust relations and to interact with other
  avatars.
---

# 📝 Interact with other Avatars

### Trust and untrust other avatars

Circles is fundamentally built around the concept of trust relations and any avatar type is able to trust and receive trust.

#### Add Trust&#x20;

Trusting an avatar means you're willing to accept Circles that have been issued by this avatar.

```javascript
await avatar.trust("0x123");
```

#### Remove Trust :&#x20;

Revokes trust from another avatar. This means you will no longer accept Circles issued by this avatar.

```javascript
await avatar.untrust("0x123");
```

### Mint personal tokens

Human avatars can mint 24 Circles per day. Call `personalMint()` to mint the max. available amount at the current point in time to your avatar address.

```javascript
await avatar.personalMint();
```

### Mint group tokens

If an avatar is a member of a group, it usually qualifies to mint group tokens. You can mint group tokens by using trusted personal tokens as collateral. With it, you can mint the same amount of group tokens.

```typescript
const groupAddress = "0x...";
const collateral = [selectedCollateral];
const amounts = [BigInt(mintAmount)];
const data = Uint8Array.of(0);
await avatar.groupMint(group.group, collateral, amounts, data);
```

### Transfer Circles

At the core, all Circles are either erc20 or erc1155 tokens and as such can be transferred from/to every wallet like any other token. However, Circles are usually transferred transitively along the trust network. That's what the SDK's `transfer()` method will do.

To transfer Circles transitively from your avatar to another, call the transfer method and specify the recipient and amount you want to send.&#x20;

```typescript
const to = "0x..";
const value = 1;
await avatar.transfer(to, value);
```

{% hint style="info" %}
In theory you can send your whole Circles balance to other avatars, however in practice your trust network has an impact on your liquidity.

Use the `avatar.getMaxTransferableAmount(to)` method determine the max. possible flow between your avatar and the recipient.
{% endhint %}

### Next steps

On the last few pages, we only worked with one avatar. Next we will learn how to query the data of arbitrary avatars and how to write custom queries.
