---
description: >-
  This section is for developers, advanced users, and those curious about the
  underlying mechanics of Circles Groups.
icon: nfc-magnifying-glass
---

# Technical Group Details

## Protocol Level Group

The most basic group construct is defined in the Hub contract.

Every group has its own CRC–token. Groups have members (defined as accounts they trust). In theory, groups can also trust other groups.

![](https://hackmd.io/_uploads/By5lRFGIlx.png)

### **Minting & Redemption**

**Minting**: If account A is a member of group G, then any account that holds A-CRC can turn them into G-CRC, at a rate 1:1.

The A-CRC enter the “vault” of the group.

**Redemption**: In turn, anybody in the network in possession of G-CRC can redeem them, at a 1:1 rate, against any CRC currently present in the vault.

![](https://hackmd.io/_uploads/r1_mRFfUle.png)

### **Groups (protocol) - Custom Policies**

In the contracts, there are three “entry” points for groups to be customised.

### **Mint Policy**

An external contract that is called as part of the minting flow. It gives permission or rejects the mint.

![](https://hackmd.io/_uploads/ryyFCFzIee.png)

### **Redemption Policy**

An external contract is part of the redemption flow. Returns a “redemption plan” that will be executed if it satisfies certain conditions.

![](https://hackmd.io/_uploads/rJf9AYMIex.png)

### **Burn Policy**&#x20;

An external contract is called when burning. Can veto a burn.

### **Custom Policies**

To register any group, all three policies must be provided; however, the implementation offers a trivial `BaseMintPolicy` That is deployed and can be referenced.

## Base Group

Base groups are groups with additional “structure” to protocol-level groups. Specifically, they are collections of contracts that are deployed per group. In addition to the default groups, they accept as inputs.

**Inputs**

* An owner
* A service address
* A Fee collection address
* A set of membership conditions

![Screenshot 2025-07-14 at 16.10.55](https://hackmd.io/_uploads/B1NblcM8lg.png)

There are also the following “auxiliary” contracts deployed for every group, which are both registered as organisations on Circles.

![](https://hackmd.io/_uploads/B1xMLxcMLeg.png)

### Base Treasury

* All funds from the minting of group tokens are forwarded to this treasury. It trusts the group.

### Base Mint Handler

* This org mirrors the group’s trust, trusting all the group members.

#### Base Groups have several advantages:

* Fee Collection allows the specification of where fees should be forwarded (although no fee collection is enforced via base groups
* The service address lets owners automate the updating of group membership
* The base treasury allows “redemption along a path”
* The mint handler lets users mint group tokens simply by sending CRC to an address (abstracting the requirement to interact with the hub contract)
* The deployment of ERC20 tokens reduces hiccups with gas estimation.
* Membership Conditions (which, unlike policies, can be updated by owners) allow sophisticated composition of group membership.

## Affiliate Group

As a member of the Circles ecosystem, you can join a primary group while also supporting and interacting with affiliate groups. Affiliates do not necessarily have to be members of groups. They are members who want to support groups through their Circles.

Choosing your affiliate group directly affects your daily CRC flow and trading options. Choose a group that aligns with your values, interests, and needs.

You're free to change your primary affiliate group at any time and be affiliated with multiple groups at the same time. To set or change your affiliate group:

1. Navigate to the group you want to select.
2. Click the **star icon** in the top right corner of the group page.

> <img src="../../.gitbook/assets/Screenshot 2025-07-15 at 10.44.25.png" alt="" data-size="original">

### Why Affiliate Groups Matter?

1. **CRC Distribution -** Your selected affiliate group receives 1/12th of the Circles tokens (CRC) you create daily, that’s 2 out of 24 CRC per day.
2. **Competition and Value Creation -** Users can freely change their affiliate group at any time. Because of this, groups are encouraged to offer valuable incentives and meaningful engagement to attract and retain members.

***

Now that you've successfully created your group and have all the technical details, we bet you're wondering what's next! In the final section, you can explore the details on group validation and crafting compelling use cases for your community.
