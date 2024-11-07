---
icon: book-open
hidden: true
---

# Whitepaper

## Circles - money for a decentralized world

{% hint style="info" %}
**Abstract**

We present Circles[^1] a currency protocol that provides a completely decentralized and radically accessible alternative and complement to fiat moneys. It has been designed around the single goal to not put any participant or institution, current or future, at an undue and systematic advantage over another, while remaining attractive as a means of exchange and store of value. It combines the best of existing fiat and crypto moneys while avoiding their weaknesses.

Circles achieves this by i) distributing the issuance rights (and the consequent seigniorage) across all participants, ii) prescribing an opinionated monetary policy that drives the fair distribution of money across generations of participants, while iii) allowing anybody to join without proof of identity.

Circles relies on existing social networks and trust relations between people and groups in place of existing political and financial structures to back the currency, safeguard against malicious parties and the undesired concentration of monetary control or extraction of profits from money issuance.

This whitepaper provides a detailed account of this new currency protocol, its motivation, and inner workings, and also makes the case for its well-behavedness, despite its radical d
{% endhint %}

## Introduction

A currency can be anything - gold coins, debt certificates, entries in a virtual ledger - that a group of people use for transacting with another. It lubricates the economy by facilitating exchange, serving as a store of value as well as a unit of account (i.e., it allows the comparison of goods by value). Yet, currencies are also a source of political and economic power. To date, all wide-spread currencies in use have been issued by political powers, enforced as legal tender, and often are the only accepted form of paying taxes in their respective locale. Control over the issuance of money, be it through rate-based or supply-based mechanisms, allows the creation of value out of thin air. Governments can use this value to finance part of their expenditures, a process known as _seigniorage,_ while it allows banks to finance parts of the lending business through the fractional reserve system.

While the creation of money out of thin air is not problematic in itself, we believe that the current mechanism is, for the following reasons:

* As with all concentration of power, the centralization of money issuance tempts exploitation by those in charge and creates an obvious target for those seeking undue influence. Having a transparent mechanism for money genesis without arbitrary degrees in place is a good in itself.
* Currently, newly created money ‘trickles down’ through financial institutions and enterprises into the population over time. Since prices and other nominal quantities take time to adjust to this new supply, financial institutions and other parties further ‘upstream’ benefit disproportionally from the value of new money, with workers seeing their real purchasing power increase only with severe delays. This is known as the _Cantillon effect._
* By construction, government-issued money puts those not served by that government at a disadvantage, in a myriad of ways. At a national level, this often hits those without means the hardest and reduces their social mobility. Internationally, it can link economies without negotiation power to the domestic monetary policy of international powers, as is evident in the case of the US Dollar system. A global world should have an _impartial_ currency that serves as a reserve and whose value is not tied to the interests or world views of any particular nation or institution.

The advent of blockchain technology has ushered in a wave of excitement and thinking around how to overcome these problems. Cryptocurrencies like Bitcoin are minted decentrally and participation is open to everyone. However, no cryptocurrency, Bitcoin or otherwise, has yet succeeded in becoming a money proper, for reasons we believe to be intrinsic to their design. For instance, in a thought experiment in which BTC becomes the new world currency, the existing money supply of over 90% of the total money supply is concentrated in the hands of the 2-3% of the population that hold BTC today, as Bitcoin admits a finite supply of BTC, much of which has already been minted. Switching to this monetary system is not in the economic interest of the 97+% who don’t yet hold BTC. Bitcoin may very well play a role as a global decentralized permissionless asset that can be used as a speculative store of value, but it will not have value stability against regular goods required to be attractive as a medium of exchange.(footnote: The recent paper [_The distributional consequences of Bitcoin_](https://papers.ssrn.com/sol3/papers.cfm?abstract\_id=4985877) by researchers at the European Central Bank makes a similar case. In a [rebuttal paper](https://www.murrayrudd.pro/challenging-bias-in-the-ecbs-bitcoin-analysis/), Bitcoin proponents state that it is not, in fact, the goal of Bitcoin (anymore) to become a global means of exchange, which to us indicates that they’d agree with our argument above)

Still, the promise of the Web3 stack as enabling a world of decentralized governance and finance stands, and, with the ability to design sophisticated currency systems using smart contracts, the question remains: _What does a currency have to look like to be adopted by people in such a multipolar, decentralized world, not by virtue of legal obligation as today, but by virtue of its attractive properties alone?_

Circles is our answer to this question. It is built on the belief that the most sustainable currency is one that by design puts no party, present or future, at an unfair advantage. For us, this implies the following requirements:

* **Universal access**: Everybody can participate, regardless of their political status.
* **Decentralized issuance and distributed seigniorage**: In a world in which money is not backed by a single authority, who should own the money printing process and receive the value created in this way? For us, the only reasonable answer is: Everybody.
* **No persistent control over the money supply**: As a social contract, for a currency to have value today, people have to believe that it will have value in the future. For this to happen, no group—such as early adopters—should be able to maintain control over a large share of the total supply : If they did, they could influence prices to their advantage and new participants would see little incentive to adopt the currency. This is a significant shortcoming of many existing cryptocurrencies, where initial users can accumulate and hold disproportionate amounts of the currency, deterring wider adoption.

This whitepaper introduces Circles, shows that it meets the above requirements, and makes the case for its economic well-behavedness as a currency despite its radical departure from money as we know it. In the following, we use “Circles” to refer to the protocol and “CRC” to denote the currency minted as part of the protocol.

## Circles

The Circles protocol is completely defined by the following simple rules:

1. **Universal Access:** _Everybody can create an account without proof of identity._
2. **Continuous issuance:** _Every account has the right to mint their own, individual CRC at a rate of 1 CRC per hour._(footnote: In the current implementation, a user has two weeks per CRC to make use of this right.)
3. **Demurrage:** _Every CRC reduces in nominal value at a rate of 7% per year.(footnote:_ (Demurrage as the property of moneys to lose value over time has been part of many alternative currency proposals, see [https://en.wikipedia.org/wiki/Demurrage\_(currency)](https://en.wikipedia.org/wiki/Demurrage\_\(currency\)). In the current implementation, The demurrage gets applied daily, starting from the time a user has the right to mint: If a user got the right to mint a given CRC $$y$$ days ago, then its current nominal value is $$0.93^{y/365.25}$$, provided, of course, that it was minted.)
4. **Rule of Trust:** _Accounts can trust one another. If account A trusts account B, then anybody in the network who holds CRC minted by B can swap them, at any time, for any CRC held by A, at a rate of 1:1._

Before diving into the details, let us briefly describe how those rules map to the requirements above: The first rule allows universal access. The second rule implies the decentralization of issuance. Now while CRC get issued by all accounts, as we will see, only those CRC minted by honest participants will carry value. As such, any seigniorage of the issuance process is equally distributed among all honest participants. The second part of rule 2 concerning the continuous issuance rate, together with rule 3, yields the monetary policy that prevents persistent control by any party over the money supply, by exerting an equilibrating force on all CRC balances. Finally, the fourth and last rule drives the mechanism by which we exclude dishonest parties from exploiting the system and from which a single fungible currency, CRC, emerges, by having a multitude of currencies minted by different participants interact in a controlled way.

In order to discuss these points in detail, for conceptual clarity it is convenient to introduce the distinction between the _micro level_ and the _macro level_ of Circles. The micro level looks at the interplay of all the many individual currencies that participants are minting and the possible states and transitions that the currency can achieve, in light of the rule of trust above. The macro level, on the other hand, looks at the emergent properties of the system, in which most of the CRC minted by honest participants become effectively fungible with another and we care about the “macroeconomic” properties of Circles.

Below, we look at these levels in turn. We begin with the macro level, since we first want to motivate our claim that, under the assumption that only honest parties participate and the currency is fully fungible, CRC is a currency that meets the requirements we outlined above. We then discuss the micro level, where we argue that this assumption will, indeed, hold. The final section introduces _Circles Sets_ and _Organizations_ as important extensions of the core of Circles that will facilitate the adoption and functioning of Circles.

<figure><img src=".gitbook/assets/Fig 1 - The micro and macro levels of Circles.png" alt=""><figcaption><p>Fig 1: The micro and macro levels of Circles</p></figcaption></figure>



## The Macro Level

For our discussion of the macro level, we assume that all participating parties are honest and that all CRC issued are fungible and valued in the economy. We ask how an economy would look if Circles was to become generally adopted.

<details>

<summary>Circles in practice </summary>

Let us go through a simple example of Circles in practice: Say you own 100 CRC at midnight yesterday and you spend 50 CRC on a cup of coffee, while you also get paid 100 CRC for a service provided. At midnight today, your current balance of 150 CRC gets reduced by demurrage, which results in 149,97 CRC. Moreover, you mint the last 24 CRC for your account, resulting in a new total of 173,97 CRC.

</details>



### **No persistent control over the money supply**

As stated above, one requirement for a currency to qualify for long-term and voluntary adoption is that it should be impossible for any small subset of people in the economy to monopolistically exploit the purchasing power over the economy’s output over an extended period of time. Of course, it can be possible for people’s purchasing power to differ significantly, and also for some people to sustain such power over longer periods, by continuing to contribute disproportionally to the economy and being correspondingly rewarded. However, what should _not_ be the case is that any subset of agents in the economy can keep others, especially future generations, from building up money holdings and a corresponding purchasing power that lets them have an adequate standard of living. For if a currency cannot guarantee this very basic premise, it fails to fulfill the primary purpose of any money: to serve as an efficient and suitable mechanism for distributing consumable goods. Consequently, it would never be accepted as a viable currency.

Circles satisfies this requirement in the following strong sense: Consider an economy and let $$N$$ be the set of actively minting users (they might have joined the economy at different times). For a subset $$\tilde{N} \subseteq N$$ of users in this economy, let $$m_{t}(\tilde{N})$$ denote their CRC balance’s share of the total money supply at time $$t$$. Then, for any such subset and any two times $$t \leq t’$$ in which no transactions happen between members in $$\tilde{N}$$ and the remainder of the economy, we have

$$
\left\lvert m_{t'}(\tilde{N}) - \frac{|\tilde{N}|}{|N|} \right\rvert \leq \left\lvert m_{t}(\tilde{N}) - \frac{|\tilde{N}|}{|N|} \right\rvert0.93^{(t' - t)}, \quad (1)
$$

where $$(t’ - t)$$ is the difference in years. This simple formula tells us that the relative holdings of the money supply for any group of people in the economy converge to their relative size in the population of economy at an exponential (albeit slow) rate, in the absence of economic activity to support their balance. Now, by way of rough approximation, we can identify $$m_t(\tilde{N})$$ with the relative purchasing power of the agents in $$\tilde{N}$$, since, in equilibrium, in which both goods and money markets clear, we expect the agents to be able to buy a share $$m_t(\tilde{N})$$ of available goods with their CRC.

This implies that regardless of how much of the available money is held by a set of users in Circles at any given time, they cannot maintain their purchasing power over time other than by continuing to be economically involved, which is exactly what we required.

Fig 2.The effective tax in Circles: This graph plots the inverse annual rate of relative change of the CRC balance, as a function of its initial balance, when a user is only minting new currency and not participating in any transactions : A user starting off with 30k CRC will have their balance grow by roughly 35%, which amounts to being taxed -35% etc. At the special point of 120804 CRC, the balance remains exactly unchanged. Balances above this threshold will overall lose in value. As the balances becomes larger, the effective tax converges to 7%, i.e. the demurrage completely dominates the negligible influx of newly minted CRC.

Another way to look at this dynamic is plotted in Figure 2, where the (inverse) rate of relative change of the CRC balance for a single account is plotted as a function of its initial balance. This rate can be understood as an effective tax resulting from the combined effects of continuous token issuance and the 7% annual depreciation of existing CRC(footnote: We should note, though, that this effective tax is not a money transfer in the sense that the CRC removed from one account are added to another account.). The graph reveals a unique equilibrium at 120804 CRC, where these opposing forces balance precisely. Accounts with balances below this threshold experience a net increase (negative tax), while those above see a net decrease (positive tax). Consequently, without transactions, each account’s balance will naturally converge to this stable value. This means that, over time, the total share of the money supply held by any group will be proportional to its size.

💡

**Why 7% demurrage (WIP)** lorem ipsum… I guess the graph with the CRC development could go in to here, motivating the 7% as one that generates a graph roughly such that a user will have reached the plateau by the time they die…

Fig.3 XXX

Finally, it might appear that this behavior of balances converging over time is some kind of radical feature embedded in the currency. However, this interpretation is misleading. Every currency exposed to inflation undergoes a similar wealth redistribution effect, a concept extensively analyzed by economists such as Keynes, Friedman, or Piketty. In fact, there is an alternative way to understand Circles, the _inflationary view_, which reveals this dynamic within a more familiar economic framework (see below).(footnote: In [an influential paper](https://www.sciencedirect.com/science/article/abs/pii/S0022053197923577), Kocherlakota argued that we can understand money as a way of keeping track of past economic activity and achievements (technically, he showed that any state that an economy using money would spontaneously reach could also be reached if instead of money there was a central memory ledger that made visible all past economic activity to all current participants). In this metaphor, we can interpret money with a wealth redistribution effect as a form of _leaking memory_: The wealth and achievements of our ancestry matter, but just like their statues are replaced over time, so the value of their money slowly fades.)

💡

**The inflationary view of Circles** There is an alternative way of formulating rules 2 and 3 for Circles that induces a purely nominal change into the currency and that we call the _inflationary view_ of Circles. In this view, no demurrage applies to coins, however, the amount of CRC that a user gets to mint increases over time. On the surface, two rule sets look quite different: For instance, the total money supply in the inflationary view grows unboundedly even for a finitely sized economy, while the ‘normal’ rules of Circles imply that for finite economies the total money supply is bounded. However, in terms of real economic implications, the two views are equivalent. In particular, the dynamic described in equation $$(1)$$ is exactly the same, since the relative share of CRC holdings, $$m_t(\tilde{N})$$, is the same in both views (see Appendix B for details).

The inflationary view of Circles is closer to money as we know it. For instance, inflation in fiat currencies tends to diminish the real purchasing power of money earned in the past, just like demurrage, with increasing wages ideally leading to stable purchasing power for current members of the economy.

### Distributed seigniorage

As discussed above, we believe that the radical decentralization of money issuance and the distribution of the seigniorage are necessary features that any economy needs to have in order to not put any party at a systematic advantage over another.

💡

**On the price stability of CRC (WIP) T**he notion of continuous issuance of CRC quickly raises doubts about the price stability of CRC. This is because the image of ‘free money flooding the market’ naturally leads to worries about ‘hyperinflation’. To address this worry, we show, in Appendix C, that CRC can have stable prices in an economically meaningful way. In particular, we use the overlapping-generations model of economist Paul Samuelson, one of the go-to toy models for modeling monetary economies, to show that a steady-state competitive equilibrium exists for CRC, in which prices remain stable. As this is relatively technical, we refer the interested reader to that section.

Now, from the rules alone, it is clear how Circles intends to address those points: It allows everybody to mint the same amount of new money and to spend this money as they wish. Recall that seigniorage is defined as the difference between the value of money printed and the costs of printing it. Hence, if all CRC minted by honest parties are the same, as we assume by virtue of their fungibility, and the costs of issuance are also distributed equally. Regarding the costs of issuance, in the case of Circles these are mostly the costs for maintaining the blockchain infrastructure and we indeed foresee that users will give away a certain percentage of their CRC to infrastructure providers, although we note that it is possible for users to become validators themselves and hence increase their effective seigniorage.

Now, an obvious question to ask is how significant the seigniorage will be. In other words: How much will people be able to buy from their share of the seigniorage? While this is a complicated question whose answer depends on factors such as the value of CRC in the economy, the distribution of balances and demands, a very simple calculation shows that the contribution of the seigniorage will be relatively small: If $$\bar{s}_t$$, denotes the average seigniorage per user and $$\bar{d}_t$$ be the average demand for money per user, both over period $$t$$, then under the assumption that the number of participants grows at constant rate $$g$$, we expect that, for most times,

$$
\frac{\bar{s}_t}{\bar{d_t}} \approx 1-\frac{r}{g}
$$

where $$r$$ is the demurrage rate in the relevant units (so 0.93 for $$t$$ in years). Hence, for Circles, at at an annual growth rate of the population of 2%, we’d expect seigniorage to contribute around 9% to a user’s money demand, on average. This also helps clarify the relationship of Circles to a Universal Basic Income. While Circles has in the past presented itself as such, it becomes clear from the above that the real economic value of the minted money is significantly below that of a UBI as commonly perceived. Of course, for individual users and in times of significant growth, the real value of seigniorage can be significantly higher. Still, we also believe that the distributed seigniorage can have an enormous compounded effect on the distribution of balances, and as a result also on levels of social inequality, in a society that adopts Circles. This is because the distribution of seigniorage not only resists the Cantillon effect discussed above, but it also dampens the notorious capital compounding effects by which the more money one has, the easier it is to make even more money (as an instance of the more general Matthew effect).

💡

**Financing and lending in Circles** As stated in the introduction, governments use seigniorage as an instrument to generate revenue. Similarly, banks can borrow money from the central bank to extend lending services to businesses and retail customers. What will happen to these instruments in a world that adopts Circles?

The simple answer is that all these instruments will continue to exist, with the difference being that the collective effectively becomes the root source of capital for the economy and hence its principal creditor: In line with the vision of decentralized finance, banks continue to provide lending services, business and organizations continue to take out loans to finance investments, however the source of funds and, in turn, the principal recipient of interest, is the collective. Of course, bonds exist already today and already today retail banks finance their loans to a large extent using their customers’ deposits. It is more the absence of central bank loans as a funding instrument and the higher resulting interest rates for depositors that will be the main difference.

It is in this sense that CRC gain somewhat a quality of governance token: Organisations compete for sources of funds, which can provided only by people. It is hence perfectly conceivable that users will pass most of the minted CRC on to various organization or funds (see also the section on Sets below).

To provide a concrete example, consider Alice, who is out of money and would like to make some money selling lemonade. Today she borrows 10 CRC from Bob, and generates generates a total revenue of 11 CRC. Tomorrow these CRC carry a nominal value of 10.99 CRC. She gives Bob his 10 CRC back and walks away with a profit of .99 CRC (or 1 CRC in yesterday’s present value). For Bob, this is an investment with an effective interest of around 7.5% p.a, since the CRC would otherwise have reduced in value.

## The Micro Level

On the macro level, we assumed that all accounts belonged to honest human agents and that all minted CRC were completely fungible, as we expect these to be emergent properties of the currency. At the micro level, the level at which the rules are enforced, we drop these assumptions. We now turn to discuss the dynamics at this level and explore under which conditions the macro level does emerge as assumed.

### Trust and trust-based swaps

Recall that according to the second rule of the Circles protocol, all CRC minted by different accounts are actually prima facie non-fungible (while those minted by the same accounts are fungible with one another). Hence, in reality, for two participants Alice and Bob, we distinguish between Alice-CRC and Bob-CRC, etc. The interaction between these individual currencies is governed by the fourth rule, the rule of trust, which we here restate:

_Accounts can trust one another. If account A trusts account B, then anybody in the network who holds CRC minted by B can swap them, at any time, for any CRC_

_held by A, at a rate 1:1._

To understand this rule, let us go through a simple example of applying it (see Fig.4). Let’s assume that Alice trusts Bob and Alice holds 5 Alice-CRC, while Carol holds 5 Bob-CRC. Then Carol can swap her Bob-CRCs with Alices’s Alice-CRCs, without requiring a signature or additional consent from Alice (this consent was given when Alice trusted Bob). Moreover, for this to work, it does not matter whether Alice trusts Carol or Bob trusts Carol. The same was possible if Alice held, for example, any other CRC instead of their own.

Fig.4: The simplest possible application of the rule of trust. If Alice trusts Bon, then any party in the network in possession of Bob-CRC can exchange them for any CRC held by Alice, both ones she minted as well as any others that she might hold.

By trusting an account, a user signals to the system that they believe that account to be an honest participant and that its currency should have purchasing power. The rule of trust above then literally puts their money where their mouth is: If Alice signals to the community that Bob is honest by trusting him, and Bob turns out to be dishonest, leading to a loss of purchasing power of Bob-CRC, then Alice essentially also loses her wealth in CRC as anybody in the system can swap any CRC that Alice holds against Bob-CRC.

Trusting is thus a serious responsibility on part of the user and we expect trust relations in the system to reflect people’s personal social connections.

### Transitive transfers

Money is useful when it allows complete strangers to efficiently conduct business. In Circles, there is no hard-coded requirement for vendors to accept any particular set of CRC in exchange for goods and services. That is, a vendor is in principle free to state, at the micro level, that a cup of coffee costs 10 CRC where these CRC have to be minted by some specific set of accounts. However, a simple argument shows that in markets with sufficient liquidity of CRC that are trusted by this vendor, it is sufficient (albeit not necessary) to instead obtain 10 CRC in CRC that the vendor trusts. For assume that you are able to obtain 10 trusted CRC. Then you could borrow 10 CRC from the accounts in the set specified by the vendor, pay the vendor, use the trust rule to swap the 10 CRC against the 10 trusted CRC and instantly hand back that borrowed CRC. Let us formulate this important property of Circles as a principle.

❗

**Principle of payment in trusted CRC** Accounts with sufficient liquidity of CRC they trust can effectively be paid using only CRC they trust, regardless of which CRC they require for payment.

Assuming sufficient liquidity of all trusted CRC, a core concern for the ability of agents to pay one another in the system then becomes the ability to obtain trusted CRC. Fortunately, by using the rule of trust iteratively, users can obtain trusted CRC from across the whole network, by means of so-called _transitive transfers._ Consider the following example:

```jsx
Bob trusts Alice
Alice trusts Carol
Alice has 5 AliceCRC
Carol has 5 CarolCRC
Carol wants to pay Bob 5 CRC
Carol gives 5 CarolCRC to Alice and takes 5 AliceCRC from her
Carol gives 5 AliceCRC to Bob
```

In this example, Carol would like to pay Bob 5 CRC, possibly in exchange for some service. By the principle of payment in trusted CRC, she can do so by obtaining 5 CRC that are trusted by Bob. She doesn’t hold them herself but can get them from Alice using the rule of trust, since Alice happens to trust her. Transitive transfers can also involve several hops, as in the following example.

```jsx
Bob trusts Alice
Alice trusts Carol
Carol trusts Dave
Alice has 5 AliceCRC
Carol has 5 CarolCRC
Dave has 5 DaveCRC
Dave wants to pay Bob 5 CRC
Dave gives 5 DaveCRC to Carol and takes 5 CarolCRC from her
Dave gives 5 CarolCRC to Alice and takes 5 AliceCRC from her
Dave gives 5 AliceCRC to Bob
```

Note that, while both of these examples involve the transfer of different kinds of CRC along a path, at the macro level both of these appear as the direct transfer of 5 (fungible) CRC from the sender to the receiver. Also, note that not only the total amount of CRC held by intermediate parties remains constant, at the micro-level, the total amount of trusted CRC held by an intermediate party has remained the same or, at best, even increased. This is an instance of an important general property of Circles:

❗

**Conservation of Trusted Balance** In Circles, no user can reduce another user’s total balance of trusted CRC.

Of course, the example transaction above was only possible because of the existence of a chain of swaps that the sender could apply to obtain trusted CRC together with intermediate parties holdings sufficient CRC themselves (footnote: Note also that the existence of such a path is not a function merely of the totality of trust connections, but also of the balances of all owners. For instance, it is not sufficient that there exists a chain of trust from seller to buyer, since users might not hold sufficient amounts of their own currency.). As we will discuss below in more detail, we expect such chains of swaps to exist between any two honest agents in the network, based on the well-studied connectivity properties of social networks that are expressed in the famous “six degrees of separation”. Hence, _the emergence of the effective global fungibility of CRC at the macro level is hence fundamentally driven by the properties of the social network of its users._

While we avoid technicalities in this text, it will prove convenient to introduce some basic notation around transitive transfers for what follows: Let $$S$$ denote the _state_ of the Circles network at a given time (which consists of the enumeration of all existing accounts $$N$$, the trust relations between those accounts and the specification of the balances $$B$$ that each account holds of each of the individual CRC currencies ). For two states, we write $$S \to S’$$ if there exists a sequence of transitive transfers that take an initial state $$S$$ to a final state $$S’$$. Moreover, for any two disjoint subsets of accounts $$N_s, N_r \subseteq N$$, we write $$B(N_s \to N_r|S)$$ to denote the total amount of CRC, trusted by least one account in $$N_r$$, that are held by accounts in $$N_s$$, in state $$S$$. We can then define

$$
T(N_s \to N_r|S) := \max_{S': S \to S'} B(N_s \to N_r|S')
$$

That is, $$T(N_s \to N_r|S)$$ is the maximal amount of CRC, trusted by at least one node in $$N_r$$, that nodes in $$N_s$$ can obtain by means of transitive transfers. We’ll call this quantity the _achievable accepted balance of_ $$N_s$$ _for_ $$N_r$$ _from_ $$S$$_._

### Resistance against malicious parties

At this point, we can address a question that some readers may have: Why not simply issue fungible CRCs from the start and do without the notion of trust and the trust rule altogether, like any other currency? The simple answer is: So that we can allow universal access to Circles without requiring proof of identity, while distributing the right of issuance across the user base.

Since anyone can join the system and start minting, an obvious idea to hack the currency is to create multiple accounts and thereby mint multiple times the amount of honest accounts. Such behavior would allow a malicious user to increase their wealth at the expense of honest users, by driving up prices. This is called a Sybil attack in computer security terms.

Trust and the rule of trust address this hack. To begin with, the latter implies that the extent to which a malicious party can get a foot into the economy of Circles is limited by the extent to which members of this network trust the accounts of this party. Let us illustrate this with an example first.

```jsx
Bob trusts Alice
Alice makes a fake account FakeAlice and trusts that account
Alice has 5 AliceCRC
FakeAlice has 5 FakeCRC
Alice wants to buy something worth 5 CRC from Bob using FakeCRC
FakeAlice gives 5 FakeCRC to Alice and takes 5 AliceCRC from her
FakeAlice gives 5 AliceCRC to Bob
```

In this example, Alice holds a fake second account, which means she is able to issue a total of 2 CRC per hour. Bob trusts Alice and thereby has some exposure to this malicious network, however since he only trusts one account and not both, Alice can at most pump 1 CRC per hour into the economy, using Bob as a gateway, while the coins minted by her second, fake account, are immobile and essentially useless. It is in this way that Alice’s effective issuance rate remains at 1 CRC/h.

The above is an instance of the following more general property of the Circles:

🛑

**Soft Sybil resistance** Let $$M$$ be a set of accounts controlled by a malicious party and let $$N$$ be the remainder of the network. Let $$N_M$$ denote the subset of nodes in $$N$$ that trust some node in $$M$$ (the “boundary” between $$N$$and $$M$$). Then, for any state $$S$$, the achievable accepted balance of $$M$$ for $$N$$ cannot exceed the accepted balance than is held by nodes in $$N_M$$,

$$
T(M \to N|S) \leq B(N_M \to N|S)
$$

Fig.5: Visual depiction of Soft Sybil resistance: The potential for a network of malicious nodes (red cluster) to “steal” valuable CRC from the remainder of the network (green cluster) is limited by the total holdings of ‘green’ CRC held by nodes at the boundary between the two clusters that trust the malicious party (orange cluster).

Since nodes in $$M$$ require trusted CRC in order to have any economic impact on $$N$$, the above limits this impact by the degree to which $$M$$ is trusted by members of $$N$$. We call this property _soft Sybil resistance_ as it does not provide full resistance against malicious attacks in a security systems sense. Essentially, it relies on the assumption that malicious parties have no friends, which unfortunately is empirically poorly supported. Moreover, it is also important to stress that trust can be revoked at any time, so that damage done by malicious parties can also be reduced once they are discovered.

### Emergence of global fungibility

We now turn to the question how fungibility between individual currencies will emerge as a property of the trust graph. To do so, it is natural to clarify what it means for the individual currencies to become fungible. From an operational point of view, we are interested in the ability of users to have the CRC they hold accepted everywhere within the part of the economy they care to transact with. As such, a natural measure of operational fungibility is the average fraction of their current holdings that users in the economy can spend. We define this formally as the _average spendable fraction_ among a subset of nodes $$\tilde{N} \subseteq N$$,

$$
ASF(\tilde{N}|S) = \frac{1}{|\tilde{N|}^2}\sum_{n, n' \in \tilde{N}} \frac{T(n \to n'|S)}{B_S(n)},
$$

where $$B_S(n) \equiv B(n \to N|S)$$ are the total CRC holdings of $$n$$ in state $$S$$. This quantity ranges from 0 to 1, with 1 indicating complete operational fungibility among the group of accounts in $$\tilde{N}$$, given state $$S$$. In order to develop an intuition for states that admit high values of ASF, consider the following simple examples (see Fig.6):

* In a network, in which every account trusts all the others, $$ASF(\tilde{N}|S)=1$$ for any subset $$\tilde{N}$$ and any state $$S$$.
* In a network, in which there is a trust path between any two accounts and $$S$$ is such that all accounts hold the same balance of their own respective currency, we have, for any subset, $$ASF(\tilde{N}|S) =1$$.
* In a network, in which everybody only trusts a single account, $$ASF(\tilde{N}|S)=1$$ if and only $$S$$ is such that only Circles of this single account are in circulation.

Fig 6: Trust graphs with global fungibility: The complete graph in which everybody trusts another achieves complete fungibility regardless of the distribution of CRC (left). The “ring graph” in which every user is connected, achieves complete fungibility if all parties hold the same amount of their own CRC (middle). The “star graph” formation, in which everybody only trusts a single user, will achieve complete fungibility whenever only CRC of this user are in circulation (right).

Of course, these are extremely simplified examples and the states of the Circles network will be highly complex. However, we believe that global fungibility will quickly converge to a sufficient level for daily economic exchange within Circles, for the following reasons:

* _High connectivity_: Intuitively, it is clear that the more independent paths connect any two parties, the higher more CRC can be sent (just like in a water system). As the trust connections will reflect the social connections of people, and based on the well-studied properties of social networks, we expect the number of independent paths between any two parties to scale with the size of the total network.
* _Local economics:_ Most users will only transact with a small subset of the network on a regular basis. We actually expect the average connectivity between users and this subset to be significantly higher than their average connectivity with the whole network.
* _Unstable bottlenecks:_ As discussed above, a transitive transfer along a path can only occur when all intermediate parties on that path hold sufficient balance. As such, some paths might become unavailable. The continuous issuance of CRC for all active accounts has the beneficial side-effect that it quickly “unblocks” any bottlenecks.
* _Diversified balances:_ The more diversified the mixture of CRC held by users, the more paths for transitive transfers are possible. In practice, we expect the rule of trust to result in users holding a diverse mix of the CRC minted mostly by parties they trust.
* _Sufficiency of low ASF:_ In practice, most users don’t spend significant fractions of their total money holdings on an everyday basis. Hence, we expect a low ASF value among honest participants to be perfectly sufficient in the sense that they will, at most times, be able to pay whatever they want without having to worry about the micro-level.

### Liquidity Clusters, Exchange Rates and Flow potentials

Above, we have looked at fungibility from an _operational_ perspective. It is just as interesting to look at it from a _token_ perspective, that is, by thinking not about what accounts can pay another, but how some CRC can be converted into others: For any two accounts $$n, n’ \in N$$ and some state $$S$$, we write

$$
n \succ_{k,S} n'
$$

to indicate that there is, in state $$S$$, at least one account that initially holds some positive amount $$k > 0$$ of $$n$$-CRC and can turn them into $$k$$ $$n’$$-CRC. Moreover, for a given state, we write $$n \sim_{k,S} n’$$ if two currencies are both convertible into another and we call a set of currencies minted by accounts $$\tilde{N} \subseteq N$$ a $$k$$-_liquidity cluster_ in $$S$$ if $$n \sim_{k,S} n’$$ for all $$n, n’ \in \tilde{N}$$. Now, it is easy to see that fungibility among a group as defined in the last section implies that all currencies are a liquidity cluster under mild additional assumptions: Consider a set $$\tilde{N}$$ of accounts and a state $$S$$ such that $$ASF(\tilde{N}|S) = 1$$. Moreover, assume that every party holds $$k$$ of their own currency. Then it immediately follows that the currencies minted by $$\tilde{N}$$ form a $$k$$-liquidity cluster, as by assumption there exists, for any two accounts in this set a series of transfers that has the first account lose $$k$$ of their own CRC holdings and gain the same amount of the latter’s currency. Conversely, the existence of liquidity clusters lets us derive lower bounds on the degree of operational fungibility of the system: For assume that $$\tilde{N}$$ forms a $$k$$-liquidity cluster for some $$S$$ and that we are given the promise that every account in this set holds at least $$k$$ of their own currency. Then one can show that

$$
ASF(\tilde{N}|S) \geq \frac{k}{\bar{B}_S(\tilde{N})},
$$

where $$\bar{B}_S(\tilde{N}) = \frac{1}{|\tilde{N}|} \sum_{n\in \tilde{N}} B_S(n)$$ are the average total token holdings of accounts in $$\tilde{N}$$.

The study of the convertibility of currencies is also interesting from the point of view of the _exchange rates_ between individual currencies that would emerge in the presence of a public exchange. Essentially, we expect that stable exchange rates, in which no party can extract arbitrage just by conducting transitive transfers will to respect the fungibility order introduced above: Let $$R(n \to n'|S)$$ denote the exchange rate between $$n$$-CRC and $$n’$$-CRC in state $$S$$, in the sense that one unit of the former can be exchanged for that many units of the latter. Then, we can show that, if $$R$$ is such that it allows no arbitrage from $$S$$, it has to be of the form

$$
R(n \to n'|S) = \frac{V(n'|S)}{V(n|S)},
$$

where $$V$$ is a state-dependent, real-valued _value_ function with the property that for any $$k>0$$,

$$
n \succ_{k,S} n' \Rightarrow V(n|S) \geq V(n'|S).
$$

This useful characterization immediately implies that currencies that form a liquidity-cluster should, in _stable_ states (in which no arbitrage is possible), exchange at a rate 1:1. Moreover, it paints a nice picture of different parts of the network as naturally separating into clusters with differing _flow potentials_ between them: Consider, for example, the graph shown in Figure 7, which depicts two “connected” sets of users (call them “Left” and “Right”) in which there is a chain of trust between all users, with a single additional one-sided trust connection going from one user in Left to one user in Right. Now, for any state $$S$$ in which each of the users in this network holds some amount of their own currency, the currencies minted by accounts in the Left and Right sets fall into a two liquidity clusters, within which, under stable conditions, they have the same value, say $$V_L$$ and $$V_R$$ respectively, and that moreover $$V_R \geq V_L$$.

Fig. 7: A trust graph with two connected clusters that are connected by a unilateral trust connection. If every user holds some of their own currency, stable exchange rates will imply that the currency minted by the right cluster will have higher value than those minted by the left cluster.

The existence of liquidity clusters and flow potentials imply the possibility that instead of a single globally fungible currency, a variety of currencies emerge, with differing but stable exchange rates. Here, we have focussed, for the sake of conceptual simplicity, on the case that global fungibility between all CRCs emerges, however this scenario is both technically and economically a possibility.

This concludes our study of the micro level of Circles. We have shown how the simple rules of Circles produces a rich structure that allows us to achieve our requirements of both providing universal access to the protocol while also distributing the rights to issue money across the user base, in such a way that one, or few, globally fungible currencies emerge at the macro level.

## Circles Sets and Organizations

Above, we have introduced and motivated the core rules of Circles. These rules are self-contained and sufficient to establish Circles as a currency that is issued by all people in the economy and that is backed by the trust connections between them. However, an economy consists of more than just individuals. We therefore introduce Sets and Organizations (which we capitalize here to distinguish them from the concepts) as two special kinds of accounts that facilitate the operation of Circles and extend the number of its use cases.

_Organizations_ are accounts that work just like human accounts, except that they don’t issue their own currency. This reflects our conviction that Circles should be a currency _minted only by the people in the economy_. In practice, shops, vendors and other non-human institutions will own Organization accounts.

_Circles Sets_ (or just _Sets_ for short) are accounts that _do_ have their own currency, however this currency is not minted like with human accounts, but can instead be created in exchange for personal CRC from selected users: Accounts trusted by Sets are called _members_ of the Set. The default minting policy allows anybody on the network to exchange any CRC minted by members, at any time and at a 1:1 rate, against newly minted Set-CRCs, where they enter in the Set’s _vault_ as collateral for the Set-CRCs and are effectively taken out of supply. Finally, anybody in possession of a Set-CRC can redeem them, at any time and at a 1:1 rate, against any CRC in the vault, which effectively burns the redeemed Set-CRCs (see Fig. 8). This minting policy combines the rule of trust with a minting mechanism, ensuring that the total supply of CRCs is not affected.

While Organizations are relatively straightforward in scope, Circles Sets are a powerful addition to Circles that serve various purposes. We now turn to briefly discuss the main ones.

Fig. 8: The base Circles Sets minting policy in action: Alice and Bob are members of the Circles Set S. Alice mints a Set-CRC by giving one of her CRC as a collateral, which is ‘locked’ into the Set’s vault. Bob, who had previously minted a Set-CRC, redeems the latter against Alice’s CRC. Throughout, the total supply of CRC that are not locked in a vault does not change.

### Currencies for shared interest groups

People naturally share interests, locations, economic needs, etc. Circles Sets give these people an instrument to organize economically in groups, using CRC as the base currency. There are many possible varieties of Sets, of which some possible ones are

* Location-based (Humans of Berlin, …)
* Requirements-based (Humans with proven identity, Active users, …)
* Community-based (Humans of Urben Gardening Group XYZ, …)
* Enterprise-based (Humans of Gnosis, …)
* Event-based (Humans of DappCon 2024, …)
* Interest-based (Humans who play Chess, …)
* Education-based (Humans of Cambridge University, …)
* Earth-based (Humans of Gaia, …)

It is important to note that there can be many different Circles Sets targeting the same segment, such as for example Proof of Humanity and WorldID, which have the same underlying admission criteria but different implementations. Since anyone can register a Circle Set and there is no central oversight or orchestration, this is only natural. Moreover, just like any other account, Sets can be trusted by both human accounts and other Set accounts. The minting mechanism for Sets then immediately implies that Set membership is effectively (albeit not by definition) transitive: If A is a member of Set S and S is a member of Set T, then A can mint T-CRC just like they were members of T. This transitivity naturally allows for groups represented by Sets to organize into sub-Sets.

What advantage do Sets like the above gain from minting their own currency? To understand this, it is important to understand that Sets can extend and build on the default minting policy described above by calling external smart contracts at various points in the minting and redemption process. In doing so, they can, for example, limit the maximal number of Set-CRC to be issued, or steer the supply dynamically as a function of the network state or other input, require exogenous collateral to be provided for minting, create lock-in against redemption or require certain forms of identification for minting or membership. Moreover, membership can be extended and revoked automatically based on certain requirements. As such, use cases include

* local and complementary currencies
* loyalty programs that require minimum levels of activity to enjoy certain benefits
* voting and funding mechanisms for DAOs
* liquidity pooling
* safety-critical applications that require highly credible collateral.

At all times, however, the base rules of Circles apply and connect the Circles Sets and their economics.

#### Active and Passive Circles Sets

Since membership in Circles Sets is defined by the Set trusting an account (rather than the other way around), and Circles places no restrictions on trusting others, we expect that users will be members of various Circles Sets without every actively seeking membership (examples include region-based Circles Sets). This might or might not be of practical interest to these users, in any case no harm can be done by such groups, since the trust is only unilateral. We call such Circles Sets that extend membership to many accounts automatically _passive_ Circles Sets and contrast them with _active_ Circles Sets, for which users have to actively engage or apply to gain and retain membership. Examples for active Circles Sets might be cooperatives, DAOs, or other enterprise-based Sets.

### The resilience-efficiency tradeoff

A second important purpose of Sets is that they provide a way for members of a Circles-based economy to navigate the tradeoff between monetary efficiency and resilience. By efficiency, we essentially mean the absence of overhead due to frictions and transaction costs transactions in operating Circles. By resilience, we mean the difficulty for malicious parties and free-riders to exploit the currency as well as the possibility to shield some parts of the economy from shocks or crises in another. The tradeoff between the two is a well-known theme in mainstream economics, especially in the macroeconomics of international finance: The use of a single currency for several markets (or to a lesser extent, by linking different markets via fixing the exchange rates of their respective domestic currencies, as in the gold standard) can reduce frictions in trade and labour mobility between these markets, increasing their overall efficiency. Yet, it also reduces the resilience of individual markets, as the lack of domestic monetary policy tools can also make a crisis in one market spill over into another in ways that could have been prevented, or at least dampened, otherwise.

The base rules of Circles are anchored very much on the resilient end of the spectrum, in that there is a single currency for each member and they are, by default, all mutually non-fungible. Since trust connections can be revoked and altered at any time, Circles provides a lot of tooling for individuals to safeguard the value of their personal CRC against shocks such as the discovery of a Sybil network.

The price Circles pays for this is its efficiency: Seemingly simple transactions at the macro level might involve a high computational effort at the micro level to identify the paths of transitive transfers. Moreover, newcomers to the system might find it difficult to quickly have their CRC accepted.

Sets address these problems, by providing a ‘fast lane’ to efficiency. To see this, consider a Setthat automatically makes every account a member that is not obviously a bot. Then all users could just turn all their individual CRCs into that Set’s CRC and immediately arrive at a fully fungible currency with very low transaction costs (even at the micro level) and quick onboarding of new joiners. Of course, the price to pay for this is the reduced resilience, since the low barrier to entry make the Set an easy target for hackers. While this is an extreme example, in practice, we expect Sets to exist at various points of the efficiency-resilience spectrum, so that users can choose a mixture of Set-CRC and individual CRC that suits their preferences and levels of risk aversion.

## Conclusion

With Circles, we have set out to propose an answer to the question: _What does a currency have to look like in order to be adopted by people in a multipolar, decentralized world, not by virtue of legal obligation, but by virtue of its attractive properties alone?_ We have argued that a currency should satisfy three necessary conditions to qualify as a good answer to this question: It should be universally accessible, its issuance mechanism should be decentralized and it should not allow the persistent control, of any group, over its money supply. We have then shown that Circles satisfies these conditions and that it does so by using the existing social trust connections between people. We have also presented some additional structures that should facilitate the adoption of Circles among groups of people with shared interests.

History has seen many proposals for currencies, mainstream and alternative, come and go. Unsurprisingly, the currencies backed and supported by those institutions with the most political and military power have stood the best chances of becoming adopted. As such, any currency proposal not originating from dominant forces has its odds against itself and Circles is no exception to this.However, the 21st century has offered a glimpse of the fundamental transformation in the technology of money unlocked by digitization and decentralization. Circles is the next evolution of this movement.

### FAQ (WIP)

* Is this a universal basic income?
* Why would anybody accept CRC?
* Do you want to replace existing money systems?
* What creates demand for CRC?
* Is this illegal?
* Why the choice of 7%?
* Why the rate of 1CRC/h?

[WP Skeleton](https://www.notion.so/WP-Skeleton-11f3b92f9cd0807e84a9cf6af8cad7d4?pvs=21)

[^1]: Circles has a long history. Originally conceived of by Martin Köppelmann in 2014, it was first launched by Gnosis in 2020, and then XXX
