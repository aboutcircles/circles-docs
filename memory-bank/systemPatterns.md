# System Patterns

## Architecture Overview

The Circles system is built on several key architectural components:

### Core Components

1. **Smart Contracts (Gnosis Chain)**
   * ERC1155-based token contracts
   * Personal avatar contracts
   * Group avatar contracts
   * Organization avatar contracts

2. **Circles SDK**
   * Core interface for interacting with Circles
   * Avatar management
   * Token operations
   * Query capabilities
   * Event subscriptions

3. **Pathfinder Service**
   * Optimal path discovery for trust connections
   * Currency exchange routing
   * Network traversal algorithms

## Design Patterns

### Token System
* **Multi-Token Standard:** Uses ERC1155 for efficient handling of multiple token types
* **Continuous Minting:** Personal tokens are minted at 1 CRC/hour
* **Demurrage:** 7% annual depreciation rate applied daily
* **Trust-Based Transfer:** Tokens can only flow through trust connections

### Avatar System
1. **Personal Avatars**
   * Individual identity representation
   * Personal token issuance
   * Trust connection management
   * Profile management

2. **Group Avatars**
   * Collective entity representation
   * Group token management
   * Member invitation system
   * Group profile handling

3. **Organization Avatars**
   * Institutional representation
   * Organizational token management
   * Hierarchical structure support

### Trust Network
* **Trust Connections:** Bidirectional trust relationships
* **Network Traversal:** Path discovery for token transfers
* **Trust Limits:** Configurable trust parameters
* **Trust Verification:** Validation of trust relationships

## Technical Implementation

### SDK Structure
* **Core Methods:** Basic interaction with Circles contracts
* **Data Methods:** Query and data retrieval functions
* **Event System:** Subscription and notification handling
* **Avatar Management:** Creation and management of different avatar types

### Query System
* **CirclesQuery Class:** Main interface for data retrieval
* **Profile Queries:** Avatar and profile information
* **Token Queries:** Balance and transaction data
* **Event Queries:** System event monitoring

### Data Flow
1. **User Interactions**
   * SDK method calls
   * Avatar operations
   * Token transfers
   * Trust management

2. **System Processing**
   * Smart contract execution
   * Pathfinder calculations
   * Event propagation
   * State updates

3. **Data Storage**
   * On-chain state
   * Profile information
   * Transaction history
   * Trust network data

## Integration Patterns

### SDK Integration
* **React Integration:** Specialized hooks and components
* **JavaScript/TypeScript Support:** Type definitions and utilities
* **Event Handling:** Subscription and callback patterns
* **Error Management:** Comprehensive error handling system

### Development Patterns
* **Avatar-First Design:** Building around avatar capabilities
* **Trust-Based Architecture:** Leveraging trust connections
* **Event-Driven Updates:** Reactive state management
* **Query-Based Data:** Efficient data retrieval patterns

## Best Practices

### Implementation Guidelines
* Use appropriate avatar types for different use cases
* Implement proper error handling
* Monitor and handle events appropriately
* Follow trust connection best practices
* Maintain proper token management

### Security Considerations
* Trust connection validation
* Token transfer verification
* Avatar authentication
* Smart contract interaction safety
* Event handling security
