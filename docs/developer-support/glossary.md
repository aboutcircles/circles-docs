---
icon: book-atlas
description: >-
  This glossary contains terms and definitions used throughout the Circles
  documentation.
---

# Glossary

### Avatar

The representation of a user or entity within the Circles network. Avatars are associated with a wallet address and a profile. There are three types in Circles v2:

*   **Human Avatar:** Represents an individual person. Can mint personal CRC tokens over time and establish trust connections. Requires an invitation to join.
*   **Group Avatar:** Represents a collective entity (community, DAO, etc.). Does not mint tokens based on time but issues its own Group Currency backed by collateral (personal CRC) provided by its members.
*   **Organization Avatar:** Represents a formal entity (business, NGO). Cannot mint tokens and relies on receiving tokens through the trust network. Subject to demurrage.

### Demurrage

A mechanism applied to Circles tokens (CRC) where their value decreases over time at a set rate (equivalent to 7% per year, applied daily). This encourages circulation and spending rather than hoarding. For Human Avatars, demurrage is typically offset by the continuous minting of new personal CRC.

### ERC-1155

An Ethereum token standard that allows a single smart contract to manage multiple token types, including both fungible (like CRC) and non-fungible tokens. Circles v2 uses ERC-1155 for its avatars and tokens, enabling efficient management of personal and group currencies.

See also:

*   [ERC-1155 Multi Token Standard on Ethereum.org](https://ethereum.org/en/developers/docs/standards/tokens/erc-1155/)

### Externally-Owned Account (EOA)

One of the two main types of Ethereum accounts, controlled directly by a private key. EOAs can initiate transactions (like sending ETH or interacting with contracts) by signing them. Most user wallets (like MetaMask) manage EOAs.

See also:

*   [Ethereum Accounts on ethereum.org](https://ethereum.org/en/developers/docs/accounts)

### Gasless Transaction (Meta-Transaction)

An Ethereum transaction where the gas fee is paid by a third party (a "relayer") instead of the user initiating the action. The user signs a message authorizing the action, and the relayer submits the actual transaction to the blockchain. This allows users to interact with dApps without needing ETH in their wallet for gas fees.

### Group Currency

A type of currency specific to a Group Avatar in Circles v2. It is minted by members who collateralize their personal CRC tokens in the group's vault. Group Currencies facilitate economic activity within the group.

### Hub Contract

The core smart contract(s) in Circles responsible for managing avatar registration, token minting (personal CRC), trust relationships, and transfers. Circles has distinct Hub Contracts for V1 and V2. The SDK interacts with these contracts.

### Network (Blockchain Network)

A system of interconnected computers (nodes) that maintain and update a shared, distributed ledger (the blockchain) according to a specific protocol (e.g., Ethereum, Gnosis Chain). Circles operates on the Gnosis Chain network.

See also:

*   [Networks on ethereum.org](https://ethereum.org/en/developers/docs/networks)

### Pathfinder

A service used by Circles to find optimal paths through the Trust Network for transferring tokens between avatars who may not directly trust each other. It calculates routes based on existing trust connections and available balances.

### Personal Currency (CRC)

The native token type for Human Avatars in Circles. Each Human Avatar continuously mints their own personal CRC tokens over time (1 CRC per hour). These tokens are subject to demurrage. Transfers rely on the Trust Network.

### Smart Account (Smart Contract Account)

An Ethereum account implemented as a smart contract, controlled by code rather than directly by a private key (though usually managed by one or more EOAs). Smart accounts can enable features like multi-signature requirements, account recovery, batch transactions, and gasless transactions. Safe{Wallet} is a popular example.

### Transaction

An action initiated on the blockchain, typically signed by an EOA, that aims to change the state of the network (e.g., transferring tokens, interacting with a smart contract). Transactions require a gas fee and must be included in a validated block.

See also:

*   [Transactions on ethereum.org](https://ethereum.org/developers/docs/transactions)

### Trust Connection / Trust Network

The core social graph mechanism in Circles. An avatar "trusts" another avatar to indicate willingness to accept their personal CRC tokens. These bidirectional or unidirectional connections form a network, enabling tokens to flow between users, even indirectly, via the Pathfinder. Trust is essential for making different personal CRC tokens effectively fungible within the network.

### Wallet

An application or interface (like MetaMask, Rabby Wallet) that allows users to manage their blockchain accounts (EOAs or Smart Accounts). Wallets hold private keys securely, enabling users to sign transactions, view balances, and interact with dApps.

See also:

*   [Ethereum Wallets on ethereum.org](https://ethereum.org/wallets)
