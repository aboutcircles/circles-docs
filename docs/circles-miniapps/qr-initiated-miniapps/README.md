# QR-initiated MiniApps

This guide provides a generic implementation pattern for QR-initiated Circles MiniApps. It focuses on the core technical building blocks required to make these flows reliable and production-safe.

{% hint style="info" %}
If you are a vibecode developer, you can simply copy the entire page using the button above and paste in your coding environment
{% endhint %}

#### QR initiated Circles MiniApps generally have the following flow:

1. Create a short-lived intent (`intentId`, `reference`, `amount`, `recipient`, `expiresAt`).
2. Encode intent reference into transfer data.
3. Build a Gnosis App deep link URL.
4. Render QR from that URL.
5. Query transfer-data events for the recipient.
6. Decode and match event `data` against your intent.
7. Verify transfer amount from receipt logs.
8. Run your business action.

#### Gnosis App Deep Link Format

For CRC transfer flows:

```
https://app.gnosis.io/transfer/{recipientAddress}/crc?amount={amountCrc}&data={urlEncodedTransferData}
```

* `recipientAddress`: expected receiving address
* `amount`: human-readable CRC amount string
* `data`: URL-encoded transfer-data payload used for deterministic matching

Example:

```
https://app.gnosis.io/transfer/0xabc...123/crc?amount=12.5&data=0x010001000b4352432d414243313233
```

#### Use the Circles SDK Transfer Data Encoder

Use SDK utils instead of custom encoding:

```ts
import { encodeCrcV2TransferData, decodeCrcV2TransferData } from "@aboutcircles/sdk-utils";
```

{% hint style="info" %}
Encoded structure:

1. `version` (1 byte)
2. `type` (2 bytes)
3. `length` (2 bytes)
4. `payload` (N bytes)
{% endhint %}

This gives consistent decoding and catches malformed data (version/length mismatch).

#### Which Data Type to Use

* `0x0001`: UTF-8 text payload
* `0x1001`: UTF-8 message + metadata payload

Recommended:

* Use `0x0001` when you only need a reference string.
* Use `0x1001` when you want message + structured metadata.

`0x0001` example:

```ts
const encoded = encodeCrcV2TransferData(["CRC-7F3A912B"], 0x0001);
// Example from tests: "Hi" => 0x01000100024869
```

`0x1001` example:

```ts
const encoded = encodeCrcV2TransferData(
  ["Payment for order", "ref=CRC-7F3A912B"],
  0x1001,
);
// Includes metadata separator 480a868a internally
```

#### Build QR URL with Encoded Data

```ts
import { encodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

function buildQrUrl(input: {
  recipientAddress: string;
  amountCrc: string;
  reference: string;
}) {
  const encodedData = encodeCrcV2TransferData([input.reference], 0x0001);
  const dataParam = encodeURIComponent(encodedData);

  return `https://app.gnosis.io/transfer/${input.recipientAddress}/crc?amount=${input.amountCrc}&data=${dataParam}`;
}
```

Richer context example:

```ts
const encodedData = encodeCrcV2TransferData(
  ["Booth payment", `ref=${reference};intent=${intentId}`],
  0x1001,
);
```

#### Detect Transactions with Transfer Data RPC Query

Use `circles_events` scoped to the expected recipient and filtered to transfer-data events.

Request:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "circles_events",
  "params": [
    "<recipientAddress>",
    null,
    null,
    ["CrcV2_TransferData"]
  ]
}
```

Detection:

1. Fetch events via `circles_events`.
2. Filter `to == recipientAddress`.
3. Decode each event `data`.
4. Match decoded reference to expected intent reference.
5. Verify amount and tx success via receipt logs.

Do not match on amount alone.

#### Decode and Extract Reference Reliably

```ts
import { decodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

type MatchResult = { matched: boolean; reason?: string };

function extractReferenceFromDecoded(payload: unknown, type: number): string | null {
  if (type === 0x0001 && typeof payload === "string") {
    return payload.trim();
  }

  if (
    type === 0x1001 &&
    payload &&
    typeof payload === "object" &&
    "metadata" in payload &&
    typeof (payload as { metadata: string }).metadata === "string"
  ) {
    const metadata = (payload as { metadata: string }).metadata;
    const m = metadata.match(/(?:^|[;\s])ref=([^;\s]+)/i);
    return m?.[1] ?? null;
  }

  return null;
}

export function dataMatchesReference(eventData: string, expectedReference: string): MatchResult {
  try {
    const decoded = decodeCrcV2TransferData(eventData);
    const found = extractReferenceFromDecoded(decoded.payload, decoded.type);

    if (!found) return { matched: false, reason: "reference not found in decoded payload" };

    return {
      matched: found.toLowerCase() === expectedReference.trim().toLowerCase(),
      reason: found,
    };
  } catch {
    return { matched: false, reason: "decode failed" };
  }
}
```

#### Verify Amount from Receipt Logs

After reference match, load tx receipt and compute actual transferred amount.

For Circles flows, sum all relevant transfer amounts for matching `from -> to` across receipt logs, then compare:

#### Idempotent Post-Confirmation Actions

```ts
await withIntentLock(intentId, async () => {
  const current = await loadIntent(intentId);
  if (current.actionStatus === "done") return;

  await performBusinessAction(current);
  await markActionDone(intentId);
});
```

#### Minimal End-to-End Example

```ts
import { encodeCrcV2TransferData, decodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

// 1) Build QR URL
const transferData = encodeCrcV2TransferData([`CRC-${intentId.slice(0, 8).toUpperCase()}`], 0x0001);
const url = `https://app.gnosis.io/transfer/${recipient}/crc?amount=${amount}&data=${encodeURIComponent(transferData)}`;

// 2) Query transfer-data events
const rpcBody = {
  jsonrpc: "2.0",
  id: 1,
  method: "circles_events",
  params: [recipient, null, null, ["CrcV2_TransferData"]],
};

// 3) Decode + match
const decoded = decodeCrcV2TransferData(event.data);
if (decoded.type !== 0x0001) return;
if (decoded.payload !== expectedReference) return;

// 4) Verify receipt amounts, then finalize
await finalizeConfirmedIntent(intentId, event.transactionHash);
```

Using `encodeCrcV2TransferData` / `decodeCrcV2TransferData` plus `circles_events` (`CrcV2_TransferData`) makes QR detection deterministic and keeps transactions properly annotated in Gnosis App for better UX.\
\
Now that the payment/user action is successfully decoded, you can proceed with your app's own business logic, be it a payout for winning a game, processing refund for a store, etc.

{% hint style="info" %}
Here are some examples you can use as a base of your own app:

* [https://github.com/aboutcircles/circles-gnosisApp-starter-kit](https://github.com/aboutcircles/circles-gnosisApp-starter-kit)
* [https://github.com/aboutcircles/merch-shop-miniapp](https://github.com/aboutcircles/merch-shop-miniapp)
{% endhint %}

