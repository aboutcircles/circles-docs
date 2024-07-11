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

{% hint style="info" %}
Groups are only available in Circles v2
{% endhint %}

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

### Full example

{% tabs %}
{% tab title="Svelte" %}
For a complete example that allows you to trust other accounts, mint and transfer Circles, check out the [Circles SDK Svelte Examples](https://github.com/aboutcircles/circles-sdk-svelte-examples) on GitHub. Specifically the [Interact with other avatars](https://github.com/aboutcircles/circles-sdk-svelte-examples/blob/master/src/routes/interact-with-other-avatars/%2Bpage.svelte) route.
{% endtab %}

{% tab title="React" %}
```tsx
import React, { useEffect, useState } from 'react';
import { Sdk, ChainConfig, SdkContractRunner, Avatar } from '@circles-sdk/sdk';
import { BrowserProvider } from 'ethers';

const chainConfig: ChainConfig = {
  pathfinderUrl: 'https://pathfinder.aboutcircles.com',
  circlesRpcUrl: 'https://rpc.helsinki.aboutcircles.com',
  v1HubAddress: '0x29b9a7fbb8995b2423a71cc17cf9810798f6c543',
};

async function getRunner(): Promise<SdkContractRunner> {
  const w: any = window;
  const browserProvider = new BrowserProvider(w.ethereum);
  const signer = await browserProvider.getSigner();
  const address = await signer.getAddress();

  return {
    runner: signer,
    address: address,
  };
}

async function getSdk() {
  const runner = await getRunner();
  return new Sdk(chainConfig, runner);
}

async function handleError<T>(fn: () => Promise<T>): Promise<T | undefined> {
  try {
    return await fn();
  } catch (e) {
    throw e;
  }
}

const CirclesInteraction = () => {
  const [avatar, setAvatar] = useState<Avatar | undefined>(undefined);
  const [sdk, setSdk] = useState<Sdk | undefined>(undefined);
  const [error, setError] = useState<Error | undefined>(undefined);
  const [trustAddress, setTrustAddress] = useState<string>('');
  const [transferRecipientAddress, setTransferRecipientAddress] = useState<string>('');
  const [transferAmount, setTransferAmount] = useState<number>(0);

  useEffect(() => {
    const initialize = async () => {
      try {
        const sdkInstance = await getSdk();
        setSdk(sdkInstance);

        const avatarInstance = await handleError(async () => {
          return await sdkInstance?.getAvatar(sdkInstance?.contractRunner.address);
        });
        setAvatar(avatarInstance);
      } catch (e) {
        setError(e as Error);
      }
    };

    initialize();
  }, []);

  const trust = async () => {
    await handleError(async () => {
      await avatar?.trust(trustAddress);
    }).catch(e => setError(e as Error));
  };

  const untrust = async () => {
    await handleError(async () => {
      await avatar?.untrust(trustAddress);
    }).catch(e => setError(e as Error));
  };

  const mint = async () => {
    await handleError(async () => {
      await avatar?.personalMint();
    }).catch(e => setError(e as Error));
  };

  const transfer = async () => {
    await handleError(async () => {
      await avatar?.transfer(transferRecipientAddress, transferAmount);
    }).catch(e => setError(e as Error));
  };

  return (
    <div>
      {error && <p style={{ color: 'darkred' }}>Error: {error.message}</p>}
      {!avatar ? (
        <p>Loading avatar...</p>
      ) : (
        <div>
          <p>
            Avatar {avatar.avatarInfo?.avatar} is {avatar.avatarInfo?.type}
          </p>
          <p>
            Add/remove trust relations:
            <br />
            <input
              type="text"
              value={trustAddress}
              onChange={(e) => setTrustAddress(e.target.value)}
              placeholder="0x.."
            />
            <button onClick={trust}>Trust</button>
            <button onClick={untrust}>Untrust</button>
          </p>
          <p>
            Mint personal tokens:
            <br />
            <button onClick={mint}>Mint</button>
          </p>
          <p>
            Transfer Circles:
            <br />
            <input
              type="text"
              value={transferRecipientAddress}
              onChange={(e) => setTransferRecipientAddress(e.target.value)}
              placeholder="0x.."
            />
            <input
              type="number"
              value={transferAmount}
              onChange={(e) => setTransferAmount(Number(e.target.value))}
              placeholder="Amount"
            />
            <button onClick={transfer}>Transfer</button>
          </p>
        </div>
      )}
    </div>
  );
};

export default CirclesInteraction;
```
{% endtab %}
{% endtabs %}
