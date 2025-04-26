---
icon: building-ngo
---

# Organization Avatars

Organization Avatars represent entities like businesses, NGOs, or other formal institutions within the Circles network. They provide a way for these entities to participate in the Circles economy, primarily by receiving payments and managing trust relationships.

Key characteristics of Organization Avatars include:

*   **No Minting:** Unlike Human Avatars, Organizations cannot mint their own personal CRC tokens over time.
*   **Subject to Demurrage:** Any CRC tokens held by an Organization Avatar are subject to the standard 7% annual demurrage.
*   **Rely on Trust:** Organizations can trust other avatars (Humans, Groups, other Organizations) to facilitate receiving payments through the trust network.
*   **Identification:** They use a profile (name, metadata stored on IPFS) for identification, similar to other avatar types.

To create a new Organization Avatar, you will use a specific function within the SDK (detailed in the next section).

**Important Prerequisite:** The wallet address used to create the Organization Avatar must *not* already be registered as any other type of avatar (Human, Group, or Organization) within the Circles v2 Hub Contract. When the creation function is called via the SDK using this address, it will be registered as an Organization Avatar.
