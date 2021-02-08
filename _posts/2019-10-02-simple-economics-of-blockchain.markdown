---
layout: post
title:  Paper Review - Economics of Blockchain
date:   2019-10-02 15:01:35 +0530
image:  12.jpg
tags:   paper-review blockchain research economics
---
Blockchain-based *Distributed Ledgers* (DLs) promise to transform the
existing financial system. The idea behind such a transformation is to
replace centralized institutions that govern the system by a
*decentralized* peer-to-peer network of nodes. I discuss a paper by {% cite
economics_of_blockchain %} that discusses economic consequences of decentralization.

The paper builds on economic theory to explain how blockchain technology
can drive markets by reducing two key costs associated with economic
activity: *cost of verification* and *cost of networking* [^1]. Due to
absence of a central clearing house or market maker, these novel
networks, when permissionless, exhibit low barriers to entry and
innovation.

Another novelty of blockchain platforms is their ability to incentivize
early contributions by developers, investors and early adopters using
native token offerings.

Economic Consequences of Decentralization {#networking_inefficiencies}
-----------------------------------------

Trade between any two parties relies on trusted intermediaries. In
exchange for their services, intermediaries charge fees and capitalize
on their ability to observe all transactions taking place within their
marketplaces. They observe an informational advantage which offers them
substantial market power and control over the participants. This results
in increased costs, reduced privacy, barriers to innovation and single
points of failure [^2].

Blockchain mitigates this reliance by distributing trust and runnning
decentralized networks of exchange. This allows for the creation of
ecosystems where the platform operators do not have an undue advantage.
Hence, blockchain platform exhibit low barriers to entry and innovation.

Cost of Verification {#cost_of_verification}
--------------------

By using costless blockchain verification over a costly verification
through an intermediary allows value to be transferred across the globe
at a minimal cost, expediting market activity. However, this does not
solve last mile problems of compliance (e.g. KYC & AML rules), linking
offline world with the real world, etc. which require an additional
cost. A lower cost of verification also makes it easier to define
property rights at a more granular scale than before, as any digital
asset (or small fraction of it) can be traded, exchanged or tracked at a
low cost on a shared ledger.

Cost of Networking
------------------

Cost of networking refers to the inefficiencies that arise due to the
market power of [incumbent players](#networking_inefficiencies). The digital marketplaces that
emerge on blockchains allow participants to make joint investments in
shared infrastructure and digital public utilities without assigning
market power to a platform operator, and are characterized by increased
competition and lower barriers to entry, and a lower privacy risk.
Therefore, blockchains also reduce the cost of networking.

Materialization Issues
----------------------

### Performance

One of the key problem with the current design of blockchains is the
[Speed-Scalability Tradeoff]({% post_url 2019-10-17-need-for-speed %}#need_for_speed) since for blockchains to be a suitable
replacement for traditional networks it will need to achieve a level of
performance (in terms of throughput, latency, cost per transaction,
etc.).

### Regulatory Frameworks

Regulatory and Compliance requirements introduce an additional overhead
in terms of [cost](#cost_of_verification) and existing frameworks need
significant revamping in order to mitigate this.

### Limited Applicability

Blockchains are not a panacea for every possible technical and market
challenge a digital ecosystem may face. Wüst and Gervais {% cite
do_you_need_blockchain %} explore suitable cases for applications of
blockchain and point out common use cases where blockchains are not a
suitable solution.

Related Work
------------

I discussed various applications of blockchains in my [previous article]({% post_url 2019-08-05-research-perspective-on-blockchain %})
and the utility of blockchains in building decentralized exchanges and
the caveats associated were discussed in detail in this [article]({% post_url
2019-07-06-flash-boys %}).

References
----------

{% bibliography --cited %}

Footnotes
---------

[^1]: Not to be confused with computer networking

[^2]: Anecdotally, the first block of Bitcoin included a reference to
    reserve bank's failure