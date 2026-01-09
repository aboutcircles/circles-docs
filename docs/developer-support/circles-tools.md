---
icon: toolbox
---

# Circles Tools

**Circles Tools** is a collection of useful tooling for the community. These tools can be used as an extension to the circles core app for niche, specific and advanced usecases.

{% embed url="https://aboutcircles.github.io/CirclesTools/" %}

{% embed url="https://pathfinder.app.aboutcircles.com/flow-visualization/" %}

Here is a detailed list of tools, you can currently use:

*   **Group Checker**

    This tool displays detailed information about Circles group memberships and token balances. It can fetch on-chain data like ERC20 supplies, fees, and balances using a smart contract and direct links to relevant explorers and swap platforms.<br>
*   **Group Creator**

    This tool allows users to create Circles groups with customizable metadata and membership conditions. It supports input of service address, group name, symbol, and optional initial trust conditions for setup.<br>
*   **Group Manager**

    This tool enables group owners to manage and update their Circles group contracts directly from the browser. Owners can view contract details, add or remove trusted members, and modify settings like service or fee collection addresses.<br>
*   **Profile Checker**

    This tool provides a detailed overview of any  address’s Circles v1 and v2  profile details like profile data, token balances, trusted relationships, and live ERC20 prices, including bot classification and expiry info.<br>
*   **Safe State & Tx Hash Checker**

    This tool allows users to inspect the state of a Gnosis Safe (owners, threshold, nonce) and compute a Safe transaction hash with full control over parameters like gas settings, operation type, and calldata. It also includes an interactive function encoder to help generate calldata from ABI inputs, and shows the full execTransaction calldata when approvals meet the threshold.<br>
*   **Trust Graph Visualizer**

    This tool visualizes the web of trust relationships in the Circles ecosystem using an interactive force-directed graph. It supports avatars, PageRank-based trust scores, dynamic expansion (recursive and top-ranked), and rich filtering (Humans, Groups, Orgs).\
    Users can explore any address, see stats and trust rankings in a sortable table, and view incoming/outgoing trust via tooltips and double-click expansion.<br>
*   **Trust Path Visualizer**

    This advanced visualization tool helps users explore trust flow paths between two Circles addresses using an interactive Sankey diagram.

    It calculates on-chain trust paths for a given Circles token transfer amount, retrieves real-time token supply capacities, and visualizes flow bottlenecks.\
    It’s ideal for understanding how CRC tokens route across trust relationships and for debugging why a transfer may or may not succeed.<br>
*   **Personal CRC Replenisher**

    This tool lets Circles users replenish their own CRC token balance by routing it through the trust network back to themselves.

    It calculates the maximum amount you can send to yourself using existing trust paths, and builds a transaction to call operateFlowMatrix on the Circles Hub.<br>
*   **Circles Backing dApp**

    This tool allows users to back their Circles tokens (CRC) with real-world assets like WBTC, WETH, GNO, or sDAI through a Safe-based flow on Gnosis Chain. It supports both Safe Apps and MetaMask wallet connections, computes a deterministic CirclesBacking contract address for any given Safe, registers appData with CowSwap to enable post-swap hooks, and guides the user through submitting two required transactions: approve() USDC.e to the factory and safeTransferFrom() CRC tokens via Hub V2. <br>
*   **LBP Starter**

    This tool helps users easily create and manage Liquidity Bootstrapping Pools (LBPs) using their Circles tokens. It allows local groups or organizations to launch an LBP by selecting a group token (CRC), choosing a paired asset (like sDAI or GNO), specifying the amount of each token, and optionally configuring advanced parameters such as weight curves, swap fees, and duration. It also supports reloading or continuing with existing LBP contracts for inspection or finalization.<br>
*   **Token Distribution Checker**

    This is an analytics tool for exploring the distribution of ERC-1155 Circles tokens. It allows users to input any Circles-compatible token address and fetch a full breakdown of its holders using the Circles Hub RPC API. The tool visually presents token holdings using a pie chart (with custom legends), paginated data tables, and balance percentages per holder. Each address is enriched with profile names (if available), explorer links, quick copy buttons, and the ability to recursively check other token distributions. Users can export the full dataset as CSV, verify the raw API data via a debug panel, and dynamically navigate token ownership. This tool is ideal for community organizers, protocol designers, or auditors who want transparency and insight into token spread, supply concentration, and holder identity.<br>
*   **Legagacy Circles Safe Creator**

    This tool is designed to help users deploy a Gnosis Safe (v1.1.1) contract instance with a single owner, tailored for legacy use within the Circles ecosystem. By connecting a MetaMask wallet, users can specify any valid Ethereum address as the owner and generate a new Safe contract using a proxy factory and master copy on Gnosis Chain. The tool handles address validation, transaction submission, and post-deployment verification to confirm that the specified address was correctly set as the owner of the Safe. It is especially useful for onboarding Circles users who need a compatible Safe to manage CRC tokens and interact with legacy dApps.<br>
*   **Onboarding Helper**

    This tool allows people to easily join a Circles group by scanner a QR code.<br>
*   **Equilibrium Analaysis**

    This tool provides a dynamic view into market efficiency and arbitrage activity within the Circles economy. It visualizes the moving average of arbitrage profit opportunities over time—expressed in USD—based on configurable time windows, helping identify trends in CRC market imbalances. Simultaneously, it offers a price equilibration analysis by showing the distribution of price spreads between liquid CRC token pairs, flagging deviations from parity using customizable thresholds, liquidity filters, and deviation types (absolute or relative). Together, these insights help traders, analysts, and protocol maintainers detect inefficiencies, monitor arbitrage potential, and assess the overall health of price discovery in the Circles ecosystem.<br>
*   [**Pathfinder App**](https://pathfinder.app.aboutcircles.com/flow-visualization/)

    This is an advanced visualization and simulation tool that computes and displays optimal token transfer paths through the Circles network graph. Users can specify a source address, destination address, and CRC value, along with custom token inclusion filters for both sender and receiver. The tool supports optional wrapped token inclusion and lets users configure visual and performance parameters—such as edge curvature, capacity gradients, node/edge labels, and tooltip details—for clarity and analysis. Upon clicking “Find Path,” the graph engine simulates feasible routes, showing flow capacity and rendering performance in real-time. This makes the Pathfinder useful for debugging trust routes, analyzing payment viability, and exploring how CRC tokens can move through the social trust graph, especially in complex multi-hop scenarios.<br>

