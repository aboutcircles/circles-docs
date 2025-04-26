---
icon: users-medical
---

# Group Avatars

Group Avatars, introduced in Circles v2, represent collective entities like communities or organizations. They follow the ERC1155 standard and allow for the creation of a shared Group Currency. Unlike personal tokens, Group Currency is not minted over time but is created by members providing their personal CRC tokens as collateral.

To create a new Group Avatar, you will use a specific function within the SDK (detailed in the next section).

**Important Prerequisite:** The wallet address used to create the Group Avatar must *not* already be registered as any other type of avatar (Human, Group, or Organization) within the Circles v2 Hub Contract. When the creation function is called via the SDK using this address, it will be registered as a Group Avatar.
