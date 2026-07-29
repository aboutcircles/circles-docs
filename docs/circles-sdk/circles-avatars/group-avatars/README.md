---
icon: users-medical
---

# Group Avatars

Group avatars are a type of avatar in the Circles ecosystem that represent collective entities rather than individual users.&#x20;

### Group Structure

Each group in Circles has:

* A unique blockchain address (similar to individual avatars)
* A specific type identifier
* An owner who has administrative control
* A treasury where tokens are stored
* A mint policy that governs token creation
* A name and symbol for its tokens
* A membership system
* Groups also maintain a count of their members and can store metadata via IPFS CIDs (Content Identifiers).

#### Groups support following CRC token operations:

* **Minting**: Groups can mint their own tokens using collateral from members
* **Redemption**: Members can redeem group tokens for the underlying collateral

### Understanding purpose of Groups

Groups within the Circles ecosystem serve several important purposes:

1. **Enabling Collective Economic Action**: Groups allow multiple individuals to create shared economic entities with their own currencies, extending Circles beyond personal tokens.
2. **Creating Community Currencies**: Groups can mint their own tokens backed by collateral, allowing communities to create currencies tailored to their specific needs.
3. **Pooling Resources**: The group treasury system allows pooling of resources from members.&#x20;
4. **Collective Governance**: Groups implement membership systems that can include conditions and expiry times.

### Base Groups and custom groups

The pages in this section describe **Base Groups** — the standard group implementation, created with `sdk.register.asGroup(...)` and managed through the `BaseGroupAvatar` surface. This is what most integrations need.

The protocol itself is not limited to Base Groups: custom group implementations with their own mint policies are also possible. Those live outside the `BaseGroupAvatar` API described here and are not covered in these pages.

