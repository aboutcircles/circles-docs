---
description: >-
  Annotating Circles transactions with arbitrary data and utilizing
  CrcV2_TransferData rpc method to build Gnosis App compatible payment links and
  QR codes with annotation.
---

# Circles x Gnosis App Starter Kit

<figure><img src="../.gitbook/assets/Screenshot 2026-01-30 at 7.40.10 PM.png" alt=""><figcaption></figcaption></figure>

{% embed url="https://github.com/aboutcircles/circles-gnosisApp-starter-kit" %}

This starter kit showcases a simple method for Circles transaction based data transfer: execute a transfer via Gnosis, then verify on-chain intent by polling Circles events for `CrcV2_TransferData` and matching `recipient + data`.

{% hint style="success" %}
Using this any arbitrary data can now be annotated with Circles transactions.
{% endhint %}

The core idea is simple:

* Treat transfer `data` as an application-level idempotency key.
* Filter event ingestion to only `CrcV2_TransferData`.
* Compare values .

This guide explains the implementation in `src/lib/circles.ts` and `src/hooks/use-payment-watcher.ts`.

### Architecture

The app pipeline is:

1. Generate a deep link: `https://app.gnosis.io/transfer/<recipient>/crc?data=<payload>&amount=<amount>`
2. User executes transfer in Gnosis.
3. Poll Circles JSON-RPC (`circles_events`) for `CrcV2_TransferData`.
4. Match event by recipient and transfer data payload.
5. Transition watcher state: `idle -> waiting -> confirmed | error`.

### Runtime and Environment

Fork and clone the repository locally.

Install and run:

```bash
npm install
npm run dev
```

{% hint style="info" %}
Environment variables:

* `NEXT_PUBLIC_CIRCLES_RPC_URL`: browser-facing RPC endpoint.
* `NEXT_PUBLIC_DEFAULT_RECIPIENT_ADDRESS`: prefilled receiver.
* `NEXT_PUBLIC_GATEWAY_ADDRESS`: fallback receiver value.

\
If no env values are supplied, `src/lib/circles.ts` defaults to staging RPC and a hardcoded recipient.
{% endhint %}

### Event Ingestion: `circles_events` with `CrcV2_TransferData`

The ingestion query is intentionally narrow:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "circles_events",
  "params": [
    "<recipient-or-cursor-or-null>",//We can also add other params like fromBlock, toBlock, etc
    null,
    null,
    ["CrcV2_TransferData"]
  ]
}
```

Implementation details:

* Recipient-scoped query is preferred when recipient is known.
* Cursor pagination is used when querying globally.
* Response events are mapped into `CirclesTransferEvent` with stable string coercion to avoid runtime type drift from RPC payloads.

### Matching Semantics (Critical Path)

`checkPaymentReceived(dataValue, minAmountCRC, recipientAddress)` returns the first matching `CirclesTransferEvent` or `null`.

Matching gates:

1. `event.to` equals  recipient address.
2. `event.data` semantically equals requested `dataValue`.

#### Data normalization

It computes candidate representations for the target string and attempts reverse decode (`hex -> utf8`) on event payloads. This reduces false negatives when wallet/indexer pipelines emit different encodings for semantically identical content.

{% hint style="info" %}
Proper encoding and decoding standards will also be introduced soon for different data types.
{% endhint %}

### Polling Loop and State Machine

`usePaymentWatcher(...)` executes recursive polling with timeout scheduling:

* Starts only when `enabled && dataValue && minAmountCRC`.
* Sets state to `waiting` before each probe.
* Calls `checkPaymentReceived(...)`.
* On hit: stores event, sets `confirmed`, stops polling.
* On exception: stores message, sets `error`, keeps control explicit.
* On cleanup: cancels loop and clears timeout.

Default interval is `5000ms`; this is configurable via `intervalMs`.

### Deep Link Construction and Correlation Key

`generatePaymentLink(recipientAddress, amountCRC, data)` emits:

```
https://app.gnosis.io/transfer/<recipient>/crc?data=<encoded-data>&amount=<amount>
```

The important field is `data`. For production, use structured unique values (`order:<uuid>`, `invoice:<tenant>:<id>`) to avoid ambiguous confirmations.\
\
This way we can implement Gnosis App integrated payments utilizing Circles.\
\
Here is a simple fun game utilizing the above discussed features:

{% embed url="https://github.com/aboutcircles/circles-gnosisApp-starter-kit/tree/king-of-hill-game" %}

