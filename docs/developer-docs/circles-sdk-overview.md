---
icon: yelp
---

# Circles SDK Overview

The [Circles SDK](https://www.npmjs.com/package/@circles-sdk/sdk) is a TypeScript library that allows you to integrate the Circles protocol into your dApp to implement avatars, profiles, token transfers, and trust connections for your use case. Behind the scenes, the Circles SDK utilizes deployed Hub Contracts on Gnosis Chain, the Pathfinder for finding trust network paths, and the profile service.

As a developer, you can start by installing the core SDK and its supported packages.

### Circles SDK Features:

```sh
npm i @circles-sdk/sdk
```

*   **Manage Signers:** Use a contract runner (like MetaMask) to sign transactions.
*   **Circles Configuration:** Set up Circles-specific configuration (contract addresses, RPC endpoints).
*   **Access Circles Data:** Query data through the Circles RPC Query API, including balances, trust relationships, and transaction histories.
*   **Support for Circles V1 and V2 Hubs:** Interact with both V1 and V2 Hub Contracts for contract-related operations.
*   **Pathfinder Client:** Use the V1 and V2 Pathfinder for finding liquid paths in the trust network to facilitate transfers.
*   **Profile Management:** Store and retrieve profiles (human, group, organization) via the Circles profile service.
*   **Avatar Management:**
    *   Retrieve avatar details by address.
    *   Register human, group, and organization avatars.
*   **Invitation Handling:** Accept invitations to join the Circles network, specifying an inviter and providing profile data.

### Circles SDK Dependency Packages

<table><thead><tr><th width="247">Package</th><th>Purpose</th><th>Why Use It</th></tr></thead><tbody><tr><td><code>@circles-sdk/abi-v1</code></td><td>Provides the ABI for Circles V1 contracts.</td><td>Interact with Circles V1 smart contracts for transfers, minting, and managing trust relationships.</td></tr><tr><td><code>@circles-sdk/abi-v2</code></td><td>Provides the ABI for Circles V2 contracts.</td><td>Interact with Circles V2 smart contracts for advanced features like group avatars and inflationary/demurrage mechanisms.</td></tr><tr><td><code>@circles-sdk/data</code></td><td>Wrapper around the Circles RPC Query API.</td><td>Query Circles-related data (balances, trust relationships, transaction histories) without direct contract interaction.</td></tr><tr><td><code>@circles-sdk/profiles</code></td><td>Manages Circles avatar profiles (human, group, organization).</td><td>Retrieve and store avatar profiles, manage trust relationships, and handle identity in the Circles ecosystem.</td></tr><tr><td><code>@circles-sdk/adapter-ethers</code></td><td>Adapter to connect Circles SDK with Ethers.js.</td><td>Leverage Ethers.js for signing and sending transactions, connecting to wallets, and interacting with contracts.</td></tr><tr><td><code>ethers</code></td><td>Library for interacting with Ethereum-based blockchains.</td><td>Simplify blockchain interactions like sending transactions, querying data, and working with smart contracts. Use ethers v6 for the Circles SDK.</td></tr><tr><td><code>multihashes</code></td><td>Handles multihash algorithms for decentralized file storage.</td><td>Ensure compatibility with IPFS and manage content-addressable storage for file integrity in decentralized systems.</td></tr></tbody></table>

{% hint style="info" %}
**Notes for Developers**

*   The SDK uses ethers.js v6 for Ethereum interactions. Make sure you are using Ethers v6 in your project as well to avoid compatibility issues.
*   Some features are specific to either v1 or v2 of the Circles protocol, such as the types of avatars that exist on the Hub Contract.
*   The SDK includes TypeScript definitions for improved developer experience and type safety.
*   When working with CIDs, the SDK provides utilities for handling both the raw bytes and base58-encoded formats.
{% endhint %}
