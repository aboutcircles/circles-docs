# Managing trust connections via Org avatar account

Organizations use the same trust surface as humans. Trust determines which avatars’ Circles they accept and who can route transfers through them.

### Trust Another Avatar

```ts
const orgAvatar = await sdk.getAvatar('0xOrgAvatar');

const receipt = await orgAvatar.trust.add('0xCollaborator');
console.log('Trust added:', receipt.hash);
```

* Default expiry is max uint96 (indefinite). Optionally pass an expiry timestamp (seconds since epoch).
* Safe runners can batch: `orgAvatar.trust.add(['0xA', '0xB'], expiry)`.

### Remove Trust

```ts
const revoke = await orgAvatar.trust.remove('0xCollaborator');
console.log('Trust removed:', revoke.hash);
```

### Inspect Trust Direction

```ts
const trusts = await orgAvatar.trust.isTrusting('0xPeer');   // org -> peer
const trustedBy = await orgAvatar.trust.isTrustedBy('0xPeer'); // peer -> org
console.log({ trusts, trustedBy });
```

### List All Trust Relations

```ts
const relations = await orgAvatar.trust.getAll();
relations.forEach((rel) => {
  console.log(`${rel.subjectAvatar} ${rel.relation} ${rel.objectAvatar}`);
});
```

* `relation` is `trusts`, `trustedBy`, or `mutuallyTrusts`. Use this to identify who the org accepts and who accepts the org for routing and payments.
