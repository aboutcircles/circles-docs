# Creation of Organizations

Organizations join Circles without invitations and don’t mint personal tokens.&#x20;

### Register the Organization

```ts
import { Sdk } from '@aboutcircles/sdk';

const sdk = new Sdk({ rpcUrl: 'https://rpc.aboutcircles.com' }, runner);

const orgAvatar = await sdk.register.asOrganization({
  name: 'My Organization',
  description: 'Circles-enabled community treasury',
  avatarUrl: '',         // optional
  previewImageUrl: '',   // optional
});

console.log('Organization avatar:', orgAvatar.address);
```

* The SDK pins the profile to the Profiles service, converts the CID to a metadata digest, and calls `hubV2.registerOrganization`.
* The returned `OrganisationAvatar` is ready for balances, transfers, trust, wrapping, group participation, etc.

#### Using an Existing Profile CID

```ts
const orgFromCid = await sdk.register.asOrganization('QmExistingProfileCid');
```

The SDK fetches the profile to validate the `name` field before registering.
