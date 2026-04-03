# QR-initiated MiniApps

This technical guide provides a generic implementation pattern for QR-initiated Circles MiniApps. It focuses on the core technical building blocks required to make these flows reliable and production-safe.

<details>

<summary>If you are not a developer, you can skip the full technical guide and use this Agents.md file to get started instead. </summary>

AGENTS.md

QR-Initiated Circles MiniApps

This document defines how an agentic or application layer should implement QR-initiated Circles MiniApps safely and deterministically.

It is intended for developers building flows where a user scans a QR code, completes a CRC transfer in Gnosis App, and the application reacts only after the payment has been correctly identified, validated, and finalized.

⸻

1. Purpose

A QR-initiated Circles MiniApp is a flow where: 1. Your app creates a short-lived payment intent. 2. That intent is encoded into Circles transfer data. 3. A Gnosis App deep link is generated and rendered as a QR code. 4. The user scans the QR code and completes the transfer. 5. Your backend watches Circles transfer-data events. 6. The app decodes the event payload and matches it to the expected intent. 7. The app verifies the actual transferred amount from the transaction receipt. 8. The app runs business logic exactly once.

This pattern is useful for: • kiosk or booth payments • store checkout flows • game payouts or claims • event ticketing or access control • merchant refunds • lightweight payment-triggered app actions

The critical goal is to make payment detection deterministic, safe, and idempotent.

⸻

2. Core Principles

Any implementation based on this flow should follow these rules:

2.1 Never rely on amount alone

Amounts are not unique identifiers. Multiple users may send the same amount, and the same user may repeat a payment. Always match on transfer data that contains a unique reference tied to your intent.

2.2 Treat transfer data as the source of intent identity

The encoded transfer data should contain a unique application-level reference that your system generated before rendering the QR code.

2.3 Verify the real transfer from the receipt

Event matching identifies a likely candidate transaction. Final confirmation must verify the amount and success using the transaction receipt and relevant transfer logs.

2.4 Make every business action idempotent

A confirmed payment may be seen more than once due to retries, polling overlap, restarts, or chain indexing behavior. Business logic must only execute once per intent.

2.5 Keep intents short-lived

QR codes should not remain valid forever. Every intent should expire and be rejected if a matching payment arrives too late.

⸻

3. High-Level Flow

Step 1: Create a short-lived intent

Before generating the QR code, create an intent object containing at least: • intentId • reference • amount • recipient • expiresAt

Suggested shape:

export type PaymentIntent = { intentId: string; reference: string; amount: string; recipient: `0x${string}`; expiresAt: string; status: "pending" | "confirmed" | "expired" | "failed"; actionStatus: "pending" | "done"; txHash?: `0x${string}`; createdAt: string; };

Step 2: Encode the reference into transfer data

Use the Circles SDK utility encoder rather than implementing custom byte serialization.

Step 3: Build the Gnosis App deep link

Construct the transfer URL using the recipient, amount, and encoded data.

Step 4: Render the QR code

Display the deep link as a QR code in your kiosk, web page, shared device flow, or point-of-sale screen.

Step 5: Query transfer-data events

Your backend should poll or watch circles\_events for CrcV2\_TransferData events scoped to the recipient address.

Step 6: Decode and match event data

Decode each event payload and extract the reference from the transfer data.

Step 7: Verify the amount from receipt logs

Once a reference match is found, load the transaction receipt and verify the actual transferred amount.

Step 8: Run business action exactly once

Use an idempotent lock so the final action cannot be executed multiple times.

⸻

4. Intent Creation

The intent is the backbone of the flow.

A strong intent should: • be created server-side • have a globally unique intentId • have a unique reference • have a clear recipient address • have the exact expected CRC amount • have a short expiration time • track both payment confirmation and business-action execution state

Example intent creation

import { randomUUID } from "crypto";

export function createIntent(input: { amount: string; recipient: `0x${string}`; ttlMs?: number; }) { const intentId = randomUUID(); const reference = `CRC-${intentId.slice(0, 8).toUpperCase()}`; const expiresAt = new Date(Date.now() + (input.ttlMs ?? 5 \* 60 \* 1000)).toISOString();

return { intentId, reference, amount: input.amount, recipient: input.recipient, expiresAt, status: "pending" as const, actionStatus: "pending" as const, createdAt: new Date().toISOString(), }; }

Recommendations • Keep TTL short, typically 2 to 10 minutes. • Use a dedicated reference rather than exposing raw database IDs. • Do not reuse references. • Store a canonical lowercase recipient address for comparison.

⸻

5. Gnosis App Deep Link Format

For CRC transfer flows, the URL format is:

https://app.gnosis.io/transfer/{recipientAddress}/crc?amount={amountCrc}\&data={urlEncodedTransferData}

Parameters • recipientAddress: the expected receiving address • amount: human-readable CRC amount string • data: URL-encoded transfer-data payload used for deterministic matching

Example

https://app.gnosis.io/transfer/0xabc...123/crc?amount=12.5\&data=0x010001000b4352432d414243313233

This deep link opens Gnosis App with a prefilled CRC transfer that includes your application-defined transfer data.

⸻

6. Use the Circles SDK Transfer Data Encoder

Use the SDK utilities instead of manually encoding bytes.

import { encodeCrcV2TransferData, decodeCrcV2TransferData, } from "@aboutcircles/sdk-utils";

Encoded structure

The transfer-data format contains: • version (1 byte) • type (2 bytes) • length (2 bytes) • payload (N bytes)

This matters because the SDK gives you: • consistent encoding • consistent decoding • validation of malformed payloads • safer parsing for version and length mismatches

Do not reinvent this format unless you fully control both sides and accept decoding risk.

⸻

7. Choosing the Right Transfer Data Type

Two useful types are:

0x0001 — UTF-8 text payload

Use this when you only need a simple reference string.

Best for: • one-off checkout references • claim IDs • deterministic intent references • the simplest possible matching logic

Example:

const encoded = encodeCrcV2TransferData(\["CRC-7F3A912B"], 0x0001);

0x1001 — UTF-8 message + metadata payload

Use this when you want a user-readable message and structured metadata.

Best for: • richer checkout descriptions • merchant display messages • combining human-readable context with machine-readable reference metadata

Example:

const encoded = encodeCrcV2TransferData( \["Payment for order", "ref=CRC-7F3A912B"], 0x1001, );

Recommendation • Use 0x0001 if all you need is a single unique reference. • Use 0x1001 if you want message + metadata while still preserving deterministic matching.

For most agent flows, 0x0001 is the safest default because the extraction logic is simpler.

⸻

8. Building the QR URL

Minimal builder

import { encodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

export function buildQrUrl(input: { recipientAddress: string; amountCrc: string; reference: string; }) { const encodedData = encodeCrcV2TransferData(\[input.reference], 0x0001); const dataParam = encodeURIComponent(encodedData);

return `https://app.gnosis.io/transfer/${input.recipientAddress}/crc?amount=${input.amountCrc}&data=${dataParam}`; }

Richer context builder

import { encodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

export function buildRichQrUrl(input: { recipientAddress: string; amountCrc: string; intentId: string; reference: string; }) { const encodedData = encodeCrcV2TransferData( \["Booth payment", `ref=${input.reference};intent=${input.intentId}`], 0x1001, );

return `https://app.gnosis.io/transfer/${input.recipientAddress}/crc?amount=${input.amountCrc}&data=${encodeURIComponent(encodedData)}`; }

Implementation notes • Always URL-encode the data field. • Keep the amount canonical and display-safe. • Do not embed sensitive server-side secrets in transfer data. • Prefer compact metadata because users may encounter the data inside wallets or logs.

⸻

9. Rendering the QR Code

QR rendering is straightforward, but a few UX details matter.

Recommended UX

Display: • the QR code • recipient label or merchant name • amount to pay • expiration timer • fallback deep link button for mobile users • a status state such as Waiting for payment...

Recommended states • pending: waiting for payment • detected: reference match found, verifying receipt • confirmed: payment confirmed • expired: intent expired • failed: payment invalid or verification failed

Operational advice • Regenerate the QR when the intent expires. • Keep the intent identifier server-side; only expose what is needed. • Consider rotating intents automatically in kiosk environments.

⸻

10. Detecting Transactions with circles\_events

Your backend should query transfer-data events scoped to the expected recipient.

RPC request

{ "jsonrpc": "2.0", "id": 1, "method": "circles\_events", "params": \[ "", null, null, \["CrcV2\_TransferData"] ] }

Detection strategy 1. Query circles\_events. 2. Only consider events that belong to the expected recipient. 3. Decode each event’s transfer data. 4. Extract the embedded reference. 5. Compare it to the expected reference for the intent. 6. If the reference matches, fetch the transaction receipt. 7. Verify transferred amount and success. 8. Mark the intent as confirmed.

Important rule

Do not finalize on event detection alone.

The event tells you that matching transfer data likely exists. Your application still needs to verify the actual transfer amount and the successful transaction outcome from the receipt.

⸻

11. Decoding and Matching Transfer Data

Use the SDK decoder for all parsing.

Reference extraction helpers

import { decodeCrcV2TransferData } from "@aboutcircles/sdk-utils";

type MatchResult = { matched: boolean; reason?: string };

function extractReferenceFromDecoded(payload: unknown, type: number): string | null { if (type === 0x0001 && typeof payload === "string") { return payload.trim(); }

if ( type === 0x1001 && payload && typeof payload === "object" && "metadata" in payload && typeof (payload as { metadata: string }).metadata === "string" ) { const metadata = (payload as { metadata: string }).metadata; const m = metadata.match(/(?:^|\[;\s])ref=(\[^;\s]+)/i); return m?.\[1] ?? null; }

return null; }

export function dataMatchesReference(eventData: string, expectedReference: string): MatchResult { try { const decoded = decodeCrcV2TransferData(eventData); const found = extractReferenceFromDecoded(decoded.payload, decoded.type);

```
if (!found) {
  return { matched: false, reason: "reference not found in decoded payload" };
}

return {
  matched: found.toLowerCase() === expectedReference.trim().toLowerCase(),
  reason: found,
};
```

} catch { return { matched: false, reason: "decode failed" }; } }

Why this matters

This gives you a deterministic match based on your app-generated reference rather than on heuristic matching such as amount-only or sender-only logic.

Matching rules

When evaluating a candidate event: • recipient must match the intent recipient • intent must not be expired • decoded payload must contain a reference • extracted reference must equal the expected reference • receipt must confirm success • receipt logs must confirm the correct transferred amount

⸻

12. Amount Verification from Receipt Logs

A matching reference is necessary but not sufficient.

After a reference match, load the transaction receipt and compute the actual transferred amount.

Why receipt verification is required

You need to confirm: • the transaction succeeded • the intended transfer actually happened • the amount is correct • the funds moved from the relevant sender path to the expected recipient

Verification approach

For Circles flows: • fetch the transaction receipt by transactionHash • parse the relevant transfer logs • sum all relevant transfer amounts for matching from -> to paths that correspond to your expected recipient • compare the total to the amount expected by the intent

Important rules • Never trust UI inputs alone. • Never trust a matched reference without checking the receipt. • If your flow allows path-based or multi-hop mechanics, compute the actual delivered amount to the final recipient rather than assuming a single direct transfer event is enough.

Suggested verification result

type ReceiptVerificationResult = { ok: boolean; actualAmount?: string; reason?: string; };

Failure examples

Reject confirmation if: • receipt status is failed • no relevant transfer logs exist • recipient does not match • actual amount is lower than expected • transaction was found after intent expiry and your business rules disallow late settlement

⸻

13. Idempotent Post-Confirmation Actions

After payment is confirmed, your application may need to perform a business action such as: • issue a refund • release digital goods • mark an order as paid • unlock a game reward • issue a ticket or access credential • trigger downstream fulfillment

This action must be idempotent.

Example

await withIntentLock(intentId, async () => { const current = await loadIntent(intentId); if (current.actionStatus === "done") return;

await performBusinessAction(current); await markActionDone(intentId); });

Why this is mandatory

Without idempotency, the same payment could trigger duplicate side effects because of: • polling overlap • webhook retries • server restarts • duplicate queue jobs • eventual consistency in your own persistence layer

Minimum requirement

Your application should have: • an intent-level lock • a persistent actionStatus • atomic state transitions where possible

⸻

14. Minimal End-to-End Example

import { encodeCrcV2TransferData, decodeCrcV2TransferData, } from "@aboutcircles/sdk-utils";

// 1) Build QR URL const transferData = encodeCrcV2TransferData( \[`CRC-${intentId.slice(0, 8).toUpperCase()}`], 0x0001, );

const url = `https://app.gnosis.io/transfer/${recipient}/crc?amount=${amount}&data=${encodeURIComponent(transferData)}`;

// 2) Query transfer-data events const rpcBody = { jsonrpc: "2.0", id: 1, method: "circles\_events", params: \[recipient, null, null, \["CrcV2\_TransferData"]], };

// 3) Decode + match const decoded = decodeCrcV2TransferData(event.data); if (decoded.type !== 0x0001) return; if (decoded.payload !== expectedReference) return;

// 4) Verify receipt amounts, then finalize await finalizeConfirmedIntent(intentId, event.transactionHash);

This is the core flow in its simplest form.

In production, you should add: • expiration checks • lowercase address normalization • retry policy • idempotency guards • structured logging • receipt verification logic • monitoring and alerting

⸻

15. Recommended Agent Responsibilities

If you are implementing this as a multi-step agent or service, split responsibilities clearly.

15.1 Intent Service

Responsible for: • creating intents • generating references • storing expiration and status • preventing duplicate active intents when needed

15.2 QR Builder

Responsible for: • encoding transfer data • constructing the Gnosis App deep link • returning the QR payload to the frontend

15.3 Event Watcher

Responsible for: • polling circles\_events • filtering candidate events • decoding transfer data • matching events to pending intents

15.4 Receipt Verifier

Responsible for: • loading transaction receipts • computing actual transferred amount • verifying success and recipient correctness

15.5 Finalization Worker

Responsible for: • acquiring intent lock • running business logic once • updating persistent state • handling retries safely

Keeping these concerns separate makes failures easier to reason about and prevents business logic from leaking into low-level detection code.

⸻

16. Suggested Persistence Model

A simple database table or collection might include:

type IntentRecord = { intentId: string; reference: string; recipient: string; amount: string; expiresAt: string; status: "pending" | "confirmed" | "expired" | "failed"; actionStatus: "pending" | "done"; txHash: string | null; matchedAt: string | null; confirmedAt: string | null; createdAt: string; updatedAt: string; };

Optional indexes: • unique index on reference • index on status • index on recipient • index on expiresAt

This helps polling workers find all active intents efficiently.

⸻

17. Failure Modes and How to Handle Them

Decode failure

Cause: • malformed or unexpected transfer data

Action: • ignore candidate event • log decode error for observability

Reference missing

Cause: • payload type not supported • metadata malformed

Action: • ignore event • log reason

Amount mismatch

Cause: • partial payment • wrong amount entered by user • transfer path produced unexpected result

Action: • do not finalize • mark intent for review or keep pending according to business rules

Expired intent

Cause: • user paid too late

Action: • reject or route to manual handling based on product policy

Duplicate processing attempt

Cause: • multiple workers or retries

Action: • lock by intentId • check actionStatus before executing business logic

Receipt unavailable temporarily

Cause: • indexing lag or RPC delay

Action: • retry with bounded backoff • do not mark confirmed until receipt verification succeeds

⸻

18. Security and Operational Guidance

Do • generate intents server-side • use unique references • keep intents short-lived • verify from receipt logs before finalizing • ensure business actions are idempotent • normalize addresses before comparing • use structured logs with intent IDs and tx hashes • monitor failed matches and decode errors

Do not • match on amount alone • trust the presence of transfer data without receipt validation • expose secrets in transfer metadata • allow unlimited lifetime QR intents • run irreversible business actions before verification completes

⸻

19. Example Logging Fields

Structured logs help a lot when debugging production payment flows.

Suggested fields: • intentId • reference • recipient • expectedAmount • txHash • eventType • decodedType • matchedReference • actualAmount • status • reason

Example:

logger.info({ intentId, reference, txHash, matchedReference, expectedAmount, actualAmount, status: "confirmed", }, "qr payment confirmed");

⸻

20. Recommended Production Sequence

A production-safe sequence looks like this: 1. Create intent in backend. 2. Persist pending intent. 3. Encode reference using encodeCrcV2TransferData. 4. Build Gnosis App transfer URL. 5. Render QR code. 6. Poll circles\_events for the recipient. 7. Decode CrcV2\_TransferData payloads. 8. Match extracted reference to pending intent. 9. Fetch transaction receipt. 10. Verify receipt success and transferred amount. 11. Mark intent as confirmed with txHash. 12. Acquire idempotent lock. 13. Run business action. 14. Mark actionStatus = done. 15. Return success to UI or downstream system.

⸻

21. When to Use 0x0001 vs 0x1001

Use 0x0001 when: • you want the simplest possible detection flow • you only need one reference string • machine matching matters more than user-facing description

Use 0x1001 when: • you want a user-readable message in the transfer data • you want metadata such as ref=...;intent=... • you need richer context for downstream parsing

In general: • start with 0x0001 • move to 0x1001 only when richer metadata adds clear product value

⸻

22. What Happens After Confirmation

Once the payment has been successfully decoded and verified, your application can continue with product-specific logic.

Examples: • pay out winnings in a game • process a refund in a store flow • mark a merch order as paid • unlock a claim flow • issue event access • trigger inventory release or fulfillment

The Circles payment flow should be treated as the trusted confirmation layer, and your business logic should sit cleanly behind that boundary.

⸻

23. Reference Implementations

You can use these repositories as practical starting points for your own app architecture: • aboutcircles/circles-gnosisApp-starter-kit • aboutcircles/merch-shop-miniapp

These are useful for understanding how to structure: • Gnosis App integration • miniapp flow design • transfer handling • real product experiences on top of Circles

⸻

24. Final Guidance

Using encodeCrcV2TransferData and decodeCrcV2TransferData together with circles\_events and CrcV2\_TransferData gives you a robust foundation for QR-initiated Circles MiniApps.

The reason this approach works well is that it separates the payment flow into clear layers: • intent creation • deterministic transfer annotation • recipient-scoped event discovery • payload decoding and reference matching • receipt-based amount verification • idempotent business-action execution

That separation makes the system easier to reason about, easier to debug, and much safer to run in real payment-triggered applications.

If you are building any QR-based Circles experience, this should be your baseline architecture.

</details>

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

