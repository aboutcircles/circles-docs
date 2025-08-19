---
icon: gift-card
---

# Wrapping and Unwrapping

Circles tokens (CRC) exist natively as ERC-1155 tokens within the Circles ecosystem. However, many external DeFi protocols and applications expect standard ERC-20 tokens. Token wrapping bridges this gap by converting ERC-1155 Circles tokens into ERC-20 compatible formats while preserving their underlying value and properties.

#### Why is Wrapping needed?

**ERC-1155 Limitations in DeFi:**

* Most AMMs only support ERC-20 tokens
* Lending protocols require ERC-20 for collateral and borrowing
* Cross-chain bridges typically support ERC-20 standard

#### Wrapper Types

**1. Demurraged Wrappers**

Maintains Circles' native economic properties with time-based value decay.

**2. Inflationary (Static) Wrappers**

Provides stable balance for traditional DeFi compatibility.

### Wrapping Circles Tokens

#### Demurraged ERC-20 Wrapping

**Functionality**: Converts ERC-1155 Circles tokens into ERC-20 tokens that maintain demurrage properties. The wrapped token balance decreases over time, preserving Circles' economic incentives.

**Use Cases:**

* Applications requiring Circles economic properties
* Time-sensitive financial instruments
* Preservation of demurrage incentives in external protocols

**Parameters:**

* `tokenAddress`: Address of the token to wrap&#x20;
* `amount`: Amount to wrap in wei (bigint)

**Returns:** Address of the deployed ERC-20 wrapper contract

```typescript
// Wrap tokens as demurraged ERC-20
const wrapperAddress = await avatar.wrapDemurrageErc20(tokenAddress, amount);
```

**Examples:**

```typescript
// Wrap personal Circles
const personalTokenAddress = avatar.address; // Personal token = avatar address
const wrapAmount = BigInt('1000000000000000000'); // 1 token in wei

const demurragedWrapper = await avatar.wrapDemurrageErc20(
  personalTokenAddress,
  wrapAmount
);

// Wrap any Circles token you hold
const anyTokenAddress = '0xAnyCirclesTokenAddress';
const demurragedWrapper = await avatar.wrapDemurrageErc20(
  anyTokenAddress,
  wrapAmount
);
```

#### Inflationary (Static) ERC-20 Wrapping

**Functionality**: Converts ERC-1155 Circles tokens into standard ERC-20 tokens with static balances. No demurrage is applied, making them suitable for traditional DeFi applications.

**Use Cases:**

* AMM liquidity provision
* Lending protocol collateral
* Cross-chain bridging
* Traditional DeFi integrations

**Parameters:**

* `tokenAddress`: Address of the token to wrap&#x20;
* `amount`: Amount to wrap in wei (bigint)

**Returns:** Address of the deployed ERC-20 wrapper contract

```typescript
// Wrap tokens as static ERC-20
const wrapperAddress = await avatar.wrapInflationErc20(tokenAddress, amount);
```

**Examples:**

```typescript
// Wrap personal Circles
const personalTokenAddress = avatar.address;
const wrapAmount = BigInt('5000000000000000000'); // 5 tokens in wei

const staticWrapper = await avatar.wrapInflationErc20(
  personalTokenAddress,
  wrapAmount
);

// Wrap received tokens from another avatar
const receivedTokenAddress = '0xReceivedTokenAddress';
const staticWrapper = await avatar.wrapInflationErc20(
  receivedTokenAddress,
  wrapAmount
);
```

### Unwrapping Circles Tokens

#### Demurraged Token Unwrapping

**Functionality**: Converts demurraged ERC-20 wrapper tokens back to native ERC-1155 Circles tokens. The unwrapping process accounts for any demurrage that occurred while wrapped.

**Parameters:**

* `wrapperTokenAddress`: Address of the ERC-20 wrapper contract
* `amount`: Amount to unwrap in wei (bigint)

**Returns:** Transaction receipt

```typescript
// Unwrap demurraged ERC-20 tokens back to ERC-1155
const unwrapTx = await avatar.unwrapDemurrageErc20(wrapperTokenAddress, amount);
```

**Example:**

```typescript
const demurragedWrapperAddress = '0xYourDemurragedWrapperAddress';
const unwrapAmount = BigInt('800000000000000000'); // 0.8 tokens

const unwrapReceipt = await avatar.unwrapDemurrageErc20(
  demurragedWrapperAddress,
  unwrapAmount
);
```

#### Static Token Unwrapping

**Functionality**: Converts static ERC-20 wrapper tokens back to native ERC-1155 Circles tokens. Since no demurrage was applied, the conversion is 1:1.

**Parameters:**

* `tokenAddress`: Original token address (the token that was wrapped)
* `amount`: Amount to unwrap in wei (bigint)

**Returns:** Transaction receipt

```typescript
// Unwrap static ERC-20 tokens back to ERC-1155
const unwrapTx = await avatar.unwrapInflationErc20(tokenAddress, amount);
```

**Example:**

```typescript
const originalTokenAddress = '0xYourOriginalTokenAddress';
const unwrapAmount = BigInt('2000000000000000000'); // 2 tokens

const unwrapReceipt = await avatar.unwrapInflationErc20(
  originalTokenAddress,
  unwrapAmount
);
```

### SDK Function Reference

| Function                        | Purpose                   | Parameters                 | Returns             |
| ------------------------------- | ------------------------- | -------------------------- | ------------------- |
| `avatar.wrapDemurrageErc20()`   | Wrap as demurraged ERC-20 | `tokenAddress`, `amount`   | Wrapper address     |
| `avatar.wrapInflationErc20()`   | Wrap as static ERC-20     | `tokenAddress`, `amount`   | Wrapper address     |
| `avatar.unwrapDemurrageErc20()` | Unwrap demurraged tokens  | `wrapperAddress`, `amount` | Transaction receipt |
| `avatar.unwrapInflationErc20()` | Unwrap static tokens      | `tokenAddress`, `amount`   | Transaction receipt |

