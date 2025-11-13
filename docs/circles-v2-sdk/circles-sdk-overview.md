---
icon: file-code
---

# Circles SDK Overview

The [Circles v2 SDK](https://www.npmjs.com/package/@aboutcircles/sdk) is a TypeScript library that allows you to integrate Circles protocol into your dApp to implement avatars, profiles, token transfers and build trust connections for your use-case. Under the hood, Circles SDK utilizes deployed hub contracts on Gnosis Chain, pathfinder for finding trust network paths and profile service and Circles RPC.

### Circles v2.0 SDK :&#x20;

To use the Circles v2 sdk, install the primary npm package

```
npm install @aboutcircles/sdk
```

Features:

* Instantiate Sdk once and work with human, organization, or group avatars through a unified interface.
* Fetch total and per-token Circles balances (demurraged and static) for any avatar.
* Add, remove, batch, and audit trust relationships across the network.
* Execute direct or advanced multi-hop transfers with automatic pathfinding and flow-matrix generation.
* Check mintable amounts, mint daily issuance, stop minting, and manage ERC20 wrapper states.
* Store, update, and resolve avatar profiles, metadata digests, and short names via the profile service.
* Register humans, organizations, and groups while handling invitation escrows end-to-end.
* Query trust graphs, avatar info, wrapper addresses, token holders, and group memberships from convenience namespaces.
* Deploy BaseGroups, configure services and membership conditions, and analyze holders or memberships.
* Compute liquidity paths, unwrap/re-wrap flows, and generate operateFlowMatrix payloads programmatically.
* Plug in EOA or Safe runners to batch transactions and execute all avatar writes with consistent error reporting.

### Dependency packages

<table><thead><tr><th width="247">Package</th><th>Purpose</th></tr></thead><tbody><tr><td>@aboutcircles/sdk-core</td><td>Core contract interactions</td></tr><tr><td>@aboutcircles/sdk-rpc</td><td>RPC client for Circles-specific methods</td></tr><tr><td>@aboutcircles/sdk-profiles</td><td>Profile management</td></tr><tr><td>@aboutcircles/sdk-types</td><td>TypeScript type definitions</td></tr><tr><td>@aboutcircles/sdk-utils</td><td>Utility functions</td></tr><tr><td>@aboutcircles/sdk-runner</td><td>Safe multisig wallet integration for executing blockchain operations with the Circles SDK.</td></tr><tr><td>@aboutcircles/sdk-transfers</td><td>Builds Circles SDK transfer payloads by combining pathfinding results with the data and RPC utilities needed to execute a transfer.</td></tr></tbody></table>
