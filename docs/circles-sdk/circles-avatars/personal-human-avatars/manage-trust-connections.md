# Manage trust connections

Trust determines which avatars will accept Circles issued by others. Circles SDK exposes `avatar.trust` helpers that batch safely (via Safe runner) or send single transactions (via EOA runners) while keeping expiry handling consistent and default under the hood.

### Add Trust

```ts
// Trust indefinitely (default expiry = max uint96)
const receipt = await avatar.trust.add('0xOtherAvatar');
console.log(receipt.hash);

// Trust with custom expiry (seconds since epoch)
const oneYear = BigInt(Math.floor(Date.now() / 1000) + 365 * 24 * 60 * 60);
await avatar.trust.add('0xOtherAvatar', oneYear);

// Trust multiple avatars at once (Safe runner only)
await avatar.trust.add(['0xA', '0xB', '0xC']);
```

* Trusting an avatar means you are willing to accept Circles they issue, transfers from them or along paths that rely on them are allowed while the trust is active.
* Passing an array batches all trust updates into one Safe transaction. EOA runners only support single addresses per call.

### Remove Trust

```ts
const receipt = await avatar.trust.remove('0xOtherAvatar');
console.log(receipt.hash);

// Batch revoke (Safe runner only)
await avatar.trust.remove(['0xA', '0xB']);
```

### Check Trust Direction

```ts
const youTrustThem = await avatar.trust.isTrusting('0xAvatar'); // you -> them
const theyTrustYou = await avatar.trust.isTrustedBy('0xAvatar'); // them -> you
console.log({ youTrustThem, theyTrustYou });
```

### Inspect All Trust Relations

```ts
const relations = await avatar.trust.getAll();

relations.forEach((relation) => {
  console.log(`${relation.subjectAvatar} ${relation.relation} ${relation.objectAvatar}`);
});
```

`getAll()` returns aggregated links where `relation` is one of:

* `trusts` – avatars you trust.
* `trustedBy` – avatars that trust you.
* `mutuallyTrusts` – avatars where trust is bidirectional.
