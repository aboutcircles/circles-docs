---
description: An avatar represents a Circles user.
---

# 👾 Using avatars

The SDK is built around the concept of avatars. An avatar is a Circles user and is used to interact with other Avatars aka Circles users.

You can get an avatar in two ways:

* Registering a new wallet at Circles via Sign Up,&#x20;
* Loading an existing avatar by it's address.

### Create a new avatar

{% hint style="info" %}
You will need a new Metamask account with some xDAI to be able to interact with contracts and follow along the steps. Make sure you configured MetaMask with the correct ChainID (Gnosis Chain or Chiado) before you continue.
{% endhint %}

### To register a new Human

You can call `registerHuman()` method to sign the connected MetaMask account up for Circles and get your avatar address.

```typescript
const avatar = await sdk.registerHuman();
console.log(avatar.avatarInfo);
```

There are different types of avatars within Circles. A `human` avatar is the most common and suitable for any person. It differentiates itself from all other avatar types in that it can mint 24 new personal Circles per day.

### Get an existing avatar

If you have the address of an existing avatar, you can get an instance by calling `sdk.getAvatar(address)`. It returns either an `AvatarInterface` instance or throws an error if the avatar can't be found.

```typescript
const avatar = await sdk.getAvatar(avatarAddress);
console.log(avatar.avatarInfo);
```

{% hint style="info" %}
You can supply any avatar address to the function, even if you don't have the key for it. However, if you just want to query the data with the `@circles-sdk/data` package you can refer to [this part of documentation](https://docs.aboutcircles.com/developer-docs/getting-started-with-the-sdk/query-data).
{% endhint %}

### Different Avatar types

Apart from `Human` Avatar you can also create `Group` and `Orangization` Avatar types. The technical details of same will be mentioned in later sections of docs. But here's how you can sign up as Group or Organization Avatar.

{% hint style="info" %}
One account can only sign up as one avatar. E.g. if an account is already signed up as human then it can't sign-up as organization.
{% endhint %}

{% tabs %}
{% tab title="Organization" %}
Organizations can be used e.g. by shop owners to receive payments in Circles. They are available in Circles v1 and v2. Organizations can't mint new Circles.

```typescript
// v1:
const avatar = await sdk.registerOrganization();
```

```typescript
// v2:
const avatar = await sdk.registerOrganizationV2();
```
{% endtab %}

{% tab title="Group" %}
The backbone of Circles are personal currencies. However, because there are so many of them, it's hard to use the personal currencies with many established protocols.

Any Group that's registered in Circles is also a Token. Group members can mint these tokens in exchange for their personal currencies, effectively making all group member's token fungible.

Groups are only available in v2.

```typescript
const avatar = await sdk.registerGroupV2();
```
{% endtab %}
{% endtabs %}

### Next steps

The next page will show you how to use the avatar instance to query essential data like the avatar's Circles balance, it's trust relations and transaction history. On the page after, we'll use it to interact with other avatars.

### Full example

{% tabs %}
{% tab title="Svelte" %}
For a complete example that registers a human or organization account, check out the [Circles SDK Svelte Examples](https://github.com/aboutcircles/circles-sdk-svelte-examples) on GitHub. Specifically the [Using avatars](https://github.com/aboutcircles/circles-sdk-svelte-examples/blob/master/src/routes/using-avatars/%2Bpage.svelte) route.
{% endtab %}

{% tab title="React" %}
```tsx
import React, { useState, useEffect } from "react";
import { Sdk, ChainConfig, SdkContractRunner, Avatar } from "@circles-sdk/sdk";
import { BrowserProvider } from "ethers";

const AvatarComponent: React.FC = () => {
    const [avatar, setAvatar] = useState<Avatar | undefined>();
    const [sdk, setSdk] = useState<Sdk | undefined>();
    const [error, setError] = useState<Error | undefined>();

    const chainConfig: ChainConfig = {
        pathfinderUrl: 'https://pathfinder.aboutcircles.com',
        circlesRpcUrl: "https://rpc.helsinki.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543"
    };

    async function getRunner(): Promise<SdkContractRunner> {
        const w: any = window;
        const browserProvider = new BrowserProvider(w.ethereum);
        const signer = await browserProvider.getSigner();
        const address = await signer.getAddress();

        return {
            runner: signer,
            address: address
        };
    }

    async function getSdk() {
        const runner = await getRunner();
        return new Sdk(chainConfig, runner);
    }

    useEffect(() => {
        const initializeSdk = async () => {
            const sdkInstance = await getSdk();
            setSdk(sdkInstance);
        };

        initializeSdk();
    }, []);

    const registerAvatar = async () => {
        setError(undefined);
        try {
            const avatarInstance = await sdk?.registerHuman();

            // If you want to sign up an organization:
            // const avatarInstance = await sdk?.registerOrganization();

            setAvatar(avatarInstance);
        } catch (e) {
            setError(e as Error);
        }
    };

    const loadAvatar = async () => {
        setError(undefined);
        try {
            const avatarInstance = await sdk?.getAvatar(sdk?.contractRunner.address);
            setAvatar(avatarInstance);
        } catch (e) {
            setError(e as Error);
        }
    };

    return (
        <div>
            {error && (
                <p style={{ color: "darkred" }}>
                    Error: {error.message}
                </p>
            )}
            {!avatar ? (
                <div>
                    <button onClick={registerAvatar}>
                        Register Human
                    </button>
                    <button onClick={loadAvatar}>
                        Load Avatar
                    </button>
                </div>
            ) : (
                <p>
                    Avatar {avatar.avatarInfo?.avatar} is {avatar.avatarInfo?.type}
                </p>
            )}
        </div>
    );
};

export default AvatarComponent;
```
{% endtab %}
{% endtabs %}
