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
