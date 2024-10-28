---
icon: book-atlas
description: >-
  This glossary contains terms and definitions used throughout the Circles
  documentation.
---

# Glossary

### ERC-1155

ERC-1155 is an Ethereum token standard that enables the efficient transfer and bundling of multiple fungible and non-fungible tokens in a single transaction. This multi-token standard allows for the creation of complex token systems, such as those used in gaming or supply chain management, where different types of tokens need to be managed simultaneously.

The standard introduces a new set of functions, including `safeTransferFrom`, `safeBatchTransferFrom`, and `balanceOfBatch`, which allow for the transfer and querying of multiple token balances in a single call. This reduces gas costs and simplifies token management compared to using multiple ERC-20 or ERC-721 contracts.

ERC-1155 tokens are identified by a unique combination of an address and an ID, allowing for the creation of an unlimited number of token types within a single contract. The standard also includes an optional metadata extension, enabling developers to associate additional information, such as images or descriptions, with each token type.

See also:&#x20;

* [ERC-1155 Multi Token Standard on Ethereum.org](https://ethereum.org/en/developers/docs/standards/tokens/erc-1155/)

### Externally-Owned Account <a href="#externally-owned-account" id="externally-owned-account"></a>

An externally-owned account (also known as EOA) is one of the two types of Ethereum accounts. A private key controls it; it has no code, and users can send messages by creating and signing Ethereum transactions.

See also:

* [Ethereum Accounts](https://ethereum.org/en/developers/docs/accounts) on ethereum.org
* [Ethereum Whitepaper](https://ethereum.org/en/whitepaper/#ethereum-accounts) on ethereum.org

### Gasless Transaction <a href="#gasless-transaction" id="gasless-transaction"></a>

Gasless transactions (also known as meta-transactions) are Ethereum transactions that are executed by a third party called [relayer](https://docs.safe.global/glossary#relayer) on behalf of a [smart account](https://docs.safe.global/glossary#smart-account) to abstract the use of gas. Users must sign a message (instead of the transaction itself) with information about the transaction they want to execute. A relayer will create the Ethereum transaction, sign and execute it, and pay for the gas costs. The main benefit is that users can interact with the blockchain without holding the native token in their account.

See also:

* [Relay Kit documentation](https://docs.safe.global/sdk/relay-kit) on docs.safe.global

### Network <a href="#network" id="network"></a>

A blockchain network is a collection of interconnected computers that utilize a blockchain protocol for communication. Decentralized networks allow users to send transactions, that are processed on a distributed ledger with a consensus mechanism ensuring the batching, verification, and acceptance of data into blocks. This structure enables the development of applications without the need for a central authority or server.

See also:

* [Networks](https://ethereum.org/en/developers/docs/networks) on ethereum.org

### Owner <a href="#owner" id="owner"></a>

A Safe owner is one of the accounts that control a given Safe. Only owners can manage the configuration of a Safe and approve transactions. They can be either [externally-owned accounts](https://docs.safe.global/glossary#externally-owned-account) or [smart accounts](https://docs.safe.global/glossary#smart-account). The [threshold](https://docs.safe.global/glossary#threshold) of a Safe defines how many owners need to approve a Safe transaction to make it executable.

See also:

* [OwnerManager.sol](https://github.com/safe-global/safe-smart-account/blob/main/contracts/base/OwnerManager.sol) on github.com

### Relayer <a href="#relayer" id="relayer"></a>

A relayer is a third-party service acting as an intermediary between users' accounts and [blockchain networks](https://docs.safe.global/glossary#network). It executes transactions on behalf of users and covers the associated execution costs, which may or may not be claimed.

See also:

* [What's Relaying?](https://docs.gelato.network/developer-services/relay/what-is-relaying) on docs.gelato.network

### Safe Wallet <a href="#safe-apps" id="safe-apps"></a>

Safe is a smart contract wallet that requires a minimum number of people to approve a transaction before it can occur (M-of-N). If for example you have 3 main stakeholders in your business, you are able to set up the wallet to require approval from 2 out of 3 (2/3) or all 3 people before the transaction is sent. This assures that no single person could compromise the funds.

See also:

* [What is Safe?](https://help.safe.global/en/articles/40869-what-is-safe)

### Smart Account <a href="#smart-account" id="smart-account"></a>

A smart account (also known as a smart contract account) leverages the programmability of smart contracts to extend its functionality and improve its security in comparison with [externally-owned accounts](https://docs.safe.global/glossary#externally-owned-account). Smart accounts are controlled by one or multiple externally-owned accounts or other smart accounts, and all transactions have to be initiated by one of those.

Some common features that smart accounts offer to their users are:

* Multi-signature scheme
* Transaction batching
* Account recovery
* [Gasless transactions](https://docs.safe.global/glossary#gasless-transaction)

Safe is one of the most trusted implementations of a smart account.

### Transaction <a href="#transaction" id="transaction"></a>

A transaction is an action initiated by an [externally-owned account](https://docs.safe.global/glossary#externally-owned-account) to update the state of the EVM network. Transaction objects must be signed using the sender's private key, require a fee, and be included in a validated block.

A Safe transaction is a transaction sent to a Safe Proxy contract calling the [execTransaction](https://github.com/safe-global/safe-smart-account/blob/main/contracts/Safe.sol#L104) method.

See also:

* [Transactions](https://ethereum.org/developers/docs/transactions) on ethereum.org

### Threshold <a href="#threshold" id="threshold"></a>

The threshold of a Safe account is a crucial configuration element that enables using Safe as a multi-signature smart account. It defines the number of required confirmations from the Safe owners a (Safe) transaction must have to be executable.

See also:

* [Get the threshold](https://docs.safe.global/sdk/protocol-kit/reference#getthreshold) and [change the threshold](https://docs.safe.global/sdk/protocol-kit/reference#createchangethresholdtx) of a Safe with the Safe{Core} SDK on docs.safe.global

### Wallet <a href="#wallet" id="wallet"></a>

A wallet is an interface or application that gives users control over their blockchain account. Wallets allow users to sign in to applications, read their account balance, send transactions, and verify their identity.

See also:

* [Ethereum Wallets](https://ethereum.org/wallets) on ethereum.org
