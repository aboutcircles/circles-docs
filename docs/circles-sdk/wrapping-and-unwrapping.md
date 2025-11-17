---
icon: gift-card
---

# Wrapping and Unwrapping

Circles tokens (CRC) are primarily ERC‑1155 balances, but most DeFi tooling expects ERC‑20s. Wrapping converts CRC into ERC‑20 wrappers so you can reach AMMs, lending markets, or bridges while keeping Circles' properties.

### Why wrap?

* AMMs and routers typically accept only ERC‑20 tokens.
* Lending/borrowing markets need ERC‑20 collateral and debt assets.
* Bridges and cross‑chain tools usually whitelist ERC‑20s.

### Wrapper choices

* **Demurraged (decaying)**: Keeps Circles' time‑based decay. Wrapped balances shrink as demurrage accrues. Use when external protocols should still respect Circles' economics.
* **Inflationary/static**: Balance stays fixed for ERC‑20 compatibility. Use for AMM liquidity, lending collateral, or bridging where decay would break integrations.

### Wrapping via `avatar.wrap`

Transactions require a configured runner (EOA or Safe) with the SDK.

#### Demurraged ERC‑20

Deploys (or reuses) a demurraged ERC‑20 wrapper for the avatar’s CRC and transfers the specified amount into it.

```ts
import { Sdk } from '@aboutcircles/sdk';
import { CirclesType } from '@aboutcircles/sdk-types';

const sdk = new Sdk(); 
const avatar = await sdk.getAvatar('0xYourAvatar');

const wrapAmount = BigInt('1000000000000000000'); // 1 CRC
const receipt = await avatar.wrap.asDemurraged(avatar.address, wrapAmount);

// Wrapper address 
const demurragedWrapper = await sdk.core.liftERC20.erc20Circles(
  CirclesType.Demurrage,
  avatar.address
);
```

#### Inflationary/static ERC‑20

Creates or fetches a static ERC‑20 wrapper so balances never decay while wrapped.

```ts
import { Sdk } from '@aboutcircles/sdk';
import { CirclesType } from '@aboutcircles/sdk-types';

const sdk = new Sdk();
const avatar = await sdk.getAvatar('0xYourAvatar');

const wrapAmount = BigInt('5000000000000000000'); // 5 CRC
const receipt = await avatar.wrap.asInflationary(avatar.address, wrapAmount);

const staticWrapper = await sdk.core.liftERC20.erc20Circles(
  CirclesType.Inflation,
  avatar.address
);
```

### Unwrapping back to ERC‑1155

Pass the ERC‑20 wrapper contract address you received (via the helper above or your own tracking).

#### Demurraged unwrapping

Demurrage continues while wrapped; the redeemable amount may be lower than what was deposited.

```ts
const demurragedWrapper = '0xYourDemurragedWrapper';
const unwrapAmount = BigInt('800000000000000000'); // 0.8 CRC

const receipt = await avatar.wrap.unwrapDemurraged(demurragedWrapper, unwrapAmount);
```

#### Static wrapper unwrapping

Balances are static while wrapped; unwrapping returns the same amount of CRC.

```ts
const staticWrapper = '0xYourStaticWrapper';
const unwrapAmount = BigInt('2000000000000000000'); // 2 CRC

const receipt = await avatar.wrap.unwrapInflationary(staticWrapper, unwrapAmount);
```

### Note:

* Pick **demurraged** wrappers to preserve Circles' economic incentives in external protocols; pick **static** wrappers for maximal ERC‑20 compatibility.
* You can wrap any ERC‑1155 Circles token you hold—not just your personal token—by passing its avatar address to `asDemurraged` or `asInflationary`.
* Store or fetch wrapper addresses for DeFi integrations; use `sdk.core.liftERC20.erc20Circles(type, avatar)` to look them up after deployment.
