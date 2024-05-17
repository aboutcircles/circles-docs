# ⚡ Migrating your Circles Account from V1 to V2

If you own a circle account which uses V1, you can simply upgrade it V2. It is mandatory to check both the conditions.

* The registration period is not over
* Your v1 token must be stopped by changing the state.&#x20;

```javascript
// Check if the registration period is already over
if (await sdk.isRegistrationPeriodOver()) {
  throw new Error('The registration period is already over');
}

// Check if the avatar is a V1 human and has been stopped
if (avatar.state.value !== AvatarState.V1_StoppedHuman) {
  throw new Error('You cannot register at Circles v2 because your v1 token is not stopped');
}
```

If your V1 token has not been stopped, you can change it :

```javascript
if (avatar.state.value !== AvatarState.V1_Human) {
  throw new Error(`You don't have a v1 token`);
}
const txReceipt = await avatar.stopV1();
```

Once you have stopped your V1 token, you can migrate your V1 avatar to V2 Avatar

```javascript

async function migrateToV2()
{
  const V2migrate = await avatar?.migrateAvatar("Insert CID");
  console.log(V2migrate);
}
```

### Registering for new V2 circles account :

A major difference from V1 to V2 circles account is  that in Circles V2, every human will have a profile which will be of ERC - 1155 standard. The profile will be a JSON object that will be stored on IPFS and will be identified by a CIDv0 (Content Identifier).  CID can be updated in case you would like to change your profile. The profile schema is defined in [ERC-1155 Metadata URI JSON Schema](https://eips.ethereum.org/EIPS/eip-1155#erc-1155-metadata-uri-json-schema).

We will call `registerHuman` function to register to v2 circles.

```javascript
const cidV0 = 'Qm...'; // CIDv0 of profile
const txReceipt = await avatar.registerHuman(cidV0);
```

**Verifying if your account was migrated to V2 version of Circles:**

If the registration was successful, the state of your avatar should have changed to `V1_StoppedHuman_and_V2_Human`.

```javascript
if (avatar.state.value !== AvatarState.V1_StoppedHuman_and_V2_Human) {
  throw new Error('Something went wrong');
```

Once you have successfully migrated to v2, you will be able to:

* get a new personal v2 token that you can use to mint.
* convert all of your v1 token holdings to v2 tokens as long as they're available on v2 already.
