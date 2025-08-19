---
icon: user-group
---

# Group Currencies

In addition to individual CRC currencies, Circles has introduced the concept of Group Avatars and Currencies. The idea behind Group currencies is to aggregate social-economic value amongst groups, without the geographical bounds of where groups are generated or created. Group avatar tokens can't be minted based on time, rather they depend on collateral deposits from trusted tokens.

Group participation operates through Circles' trust network rather than formal membership. To receive and accept group tokens, you need to trust the group address, which allows you to hold and transact with their tokens. The group, in turn, must trust the types of tokens it will accept as collateral for minting.

Group token minting is a permissionless process open to anyone holding tokens that the group trusts. When you want to mint group tokens, you deposit your trusted tokens (which can be personal CRC, other group tokens, or any Circles tokens) as collateral through the `groupMint()` function. The group mints new group tokens proportional to the collateral deposited, while your collateral tokens are held in the group's treasury.

This is not a direct swap but rather a collateralization mechanism - your original tokens remain in the group's treasury and can be redeemed later by burning group tokens through the `groupRedeem()` function. The current system allows minting based on any quantity of trusted Circles tokens you hold, not limited to personal CRC tokens.

The trust relationships work bidirectionally: you trust the group to accept their tokens, and the group trusts specific token types to accept them as collateral. This creates a flexible, decentralized system where group currencies can be backed by diverse collateral from across the Circles ecosystem, enabling economic coordination without geographic or membership constraints.
