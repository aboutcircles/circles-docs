---
description: >-
  Avatars provide easy to use methods to query common values like trust lists
  and balances.
---

# 📝 Avatar information

### Avatar info

The `avatarInfo` property contains the most important properties of your avatar like it's `address`, `type`, `tokenId` and `version`.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```

{% hint style="info" %}
Organization avatars don't have a `tokenId`.
{% endhint %}

### Total balance

This method returns the total Circles balance of an avatar as `number`.

```typescript
const totalBalance = await avatar.getTotalBalance();
```

### Mintable amount

Human avatars can mint 24 personal Circles per day. You can call getMintableAmount() to check how much Circles can be minted right now.

```typescript
const mintableAmount = await avatar.getMintableAmount();
```

### Trust relations

Trust relations play a critical role in Circles. A core principle is, that avatars only accept Circles that they previously trusted. Trust relations and their effects will be explained in greater detail at a later point. For now, you can think of it as a contact list.

```typescript
const trustRelations = await avatar.getTrustRelations();
```

The method returns an array of `TrustRelationRow`s, each with `subjectAvatar`, `relation` and `objectAvatar` attributes. That can be read as:

* 0x111... trusts 0x222...

where the first address is the subject, 'trusts' specifies the relation and the last address is the object. The following relations exist: `trusts`, `trustedBy`, `mutuallyTrusts`.

### Transaction history

Circles transfers, personal and group minting transactions will show up in the avatar's transaction history. You can call `getTransactionHistory(pageSize)` to get a paginated transaction history list.

```typescript
const transactionHistory = await avatar.getTransactionHistory(25);
```

The function returns a `CirclesQuery<T>` object that must be 'unrolled' by calling `queryNextPage()` to get the data. You're going to see more of this later when we use the `@circles-sdk/data` package directly.

```typescript
const transactionHistoryQuery = await avatar.getTransactionHistory(25);
const hasData = await transactionHistoryQuery.queryNextPage();
if (hasData) {
    console.log(transactionHistoryQuery.currentPage?.results);
}
```

The result rows contain `from`, `to` and `value` properties as well as (in some cases) the `tokenAddress` of the transferred token. The `currentPage`, additionally to the `results`, has the following fields: `limit`, `size`, `firstCursor`, `lastCursor`, `sortOrder`. You can use these fields e.g. to implement infinite scrolling in your application.&#x20;

{% hint style="warning" %}
The CirclesQuery class is not suitable for classic pagination at the moment because there is no way to get the total result count.
{% endhint %}

### Next steps

On the next page we'll show how you can trust other avatars, mint personal Circles and transfer them.

Following that, you'll learn how to subscribe to events and how to query the other data that's available from the Circles RPC endpoint.

### Full example

{% tabs %}
{% tab title="Svelte" %}
For a complete example that queries the balance, mintable amount, trust relations and transaction history, check out the [Circles SDK Svelte Examples](https://github.com/aboutcircles/circles-sdk-svelte-examples) on GitHub. Specifically the [Avatar information](https://github.com/aboutcircles/circles-sdk-svelte-examples/blob/master/src/routes/avatar-information/%2Bpage.svelte) route.
{% endtab %}

{% tab title="React" %}
```tsx
import React, { useState, useEffect } from "react";
import { Sdk, ChainConfig, SdkContractRunner, Avatar } from "@circles-sdk/sdk";
import { BrowserProvider } from "ethers";

const App: React.FC = () => {
  const [avatar, setAvatar] = useState<Avatar | undefined>();
  const [sdk, setSdk] = useState<Sdk | undefined>();
  const [error, setError] = useState<Error | undefined>();
  const [totalBalance, setTotalBalance] = useState<string | undefined>();
  const [mintableAmount, setMintableAmount] = useState<string | undefined>();
  const [trustRelations, setTrustRelations] = useState<any>();
  const [transactionHistory, setTransactionHistory] = useState<any>();

  const chainConfig: ChainConfig = {
    pathfinderUrl: "https://pathfinder.aboutcircles.com",
    circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
    v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
  };

  const getRunner = async (): Promise<SdkContractRunner> => {
    const w: any = window;
    const browserProvider = new BrowserProvider(w.ethereum);
    const signer = await browserProvider.getSigner();
    const address = await signer.getAddress();

    return {
      runner: signer,
      address: address,
    };
  };

  const getSdk = async () => {
    const runner = await getRunner();
    return new Sdk(chainConfig, runner);
  };

  useEffect(() => {
    const initializeSdk = async () => {
      const sdkInstance = await getSdk();
      setSdk(sdkInstance);
    };

    initializeSdk();
  }, []);

  const loadAvatar = async () => {
    setError(undefined);
    try {
      const loadedAvatar = await sdk?.getAvatar(sdk?.contractRunner.address);

      const [balance, mintAmount, trustRels, txnHistory] = await Promise.all([
        loadedAvatar.getTotalBalance(),
        loadedAvatar.getMintableAmount(),
        loadedAvatar.getTrustRelations(),
        loadedAvatar.getTransactionHistory(10),
      ]);

      setTotalBalance(balance);
      setMintableAmount(mintAmount);
      setTrustRelations(trustRels);
      setTransactionHistory(txnHistory);
      setAvatar(loadedAvatar);
    } catch (e: any) {
      setError(e);
    }
  };

  return (
    <div>
      {error && <p style={{ color: "darkred" }}>Error: {error.message}</p>}
      {!avatar ? (
        <button onClick={loadAvatar}>Load avatar</button>
      ) : (
        <>
          <p>
            Avatar {avatar.avatarInfo.avatar} is {avatar.avatarInfo.type}
          </p>
          <p>Circles balance: {totalBalance}</p>
          <p>Mintable amount: {mintableAmount}</p>
          <div>
            Trust relations:
            <pre>{JSON.stringify(trustRelations, null, 2)}</pre>
          </div>
          <div>
            Recent transactions:
            <pre>{JSON.stringify(transactionHistory.currentPage.results, null, 2)}</pre>
          </div>
        </>
      )}
    </div>
  );
};

export default App;

```
{% endtab %}
{% endtabs %}
