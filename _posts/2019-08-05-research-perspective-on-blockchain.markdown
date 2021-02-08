---
layout: post
title:  Paper Review - Research Perspectives for Cryptocurrencies
date:   2019-08-05 15:01:35 +0530
image:  06.jpg
tags:   paper-review blockchain research
---
The article titled "SoK: Research Perspectives and Challenges for Bitcoin and
Cryptocurrencies" {% cite SoK %} provides a comprehensive overview of the current state of
research in blockchain and explores fundamental problems available for research.
In this post, I present a few takeaways from the paper.

### Design of Bitcoin

Bitcoin is designed in 3 distinct layers or systems,


#### Communication Network

Bitcoin Protocol runs on a peer to peer network on TCP without any
hierarchy among the nodes. This network allows propagation of
Transaction data as well as announcements of newly mined blocks. Some
nodes may chose to follow a stricter security policy by having stricter
validation rules from a narrow whitelist of valid transactions.


#### Consensus Mechanism

Distributed consensus is said to be achieved when all nodes decide on
the same value. This value is valid only if it was also proposed by an
honest node. Under some technical conditions, distributed consensus is
impossible even with a single faulty node as proved in
Fischer-Lynch-Patterson Impossibility.

Bitcoin achieves consensus by providing incentive to the nodes to behave
honestly via mining rewards  {% cite BBP %}.

Consensus is achieved on the state which only consists of scripts.


#### Scripts and Transactions

The state in Bitcoin is defined by the scripts submitted by nodes, which
contain the input and output addresses for the transactions. It is an
ad-hoc, non-Turing complete scripting language with fewer than 200
commands (called *opcodes*). Each transaction contains contains:

-   *scriptSig*: A small script which would be verified later in order
    to *redeem* the transaction.

-   *scriptPubKey*: The signature on the script's hash (SHA-256 applied
    twice), signed by the private key of previous transaction.

New Ideas
---------

### Nakomoto Consensus

The mechanism for obtaining consensus on new blocks which offers
incentives to honest nodes as well as penalizes dishonest nodes. This is
done by asking any node that wants to get the incentive to solve a
computational puzzle and propose a valid block. If the node proposes an
invalid block along with the puzzle solution, it won't receive the
reward (and hence penalized by wastage of computational resources).

After the new block is proposed, new nodes begin to propose blocks on
top of it (hence, *blockchain*). The longest chain of valid blocks is
accepted as the consensus state.

### Stability

Stability is defined as the operation of system in a manner that it
facilitates proper transactions. For Bitcoin to be stable, it's 3
sub-systems also need to be stable. The paper examines the stability of
the subsystems independently of each other.

#### Communication Networks

In the current implementation, the peer-to-peer network is not always
incentive compatible since it participants do not gain much reward from
relaying information {% cite balloons %}. There is also a possibility of DDoS
attacks against mining pools. {% cite DDOS %}


#### Consensus Mechanism

The paper suggests 5 properties as the criteria for consensus:

-   **Eventual Consensus** - At any time, all honest nodes will agree on
    the prefix chain that should be the prefix of the future prefix
    chain with a high probability.

-   **Exponential Convergence** - Probability of fork of depth $ n $ is
    $ O(2^{-n}) $ . This provides confidence that chain prefix will be a
    part of the longest chain.

-   **Liveness** - Valid transactions will continue to be included in
    the blockchain.

-   **Correctness** - Not necessary but useful for SPV clients which
    validate only proof of work and not transactions.

-   **Fairness** - Proportion of blocks mined by a miner is directly
    proportional to its computational power.

Nakamoto originally argued that Bitcoin will remain stable as long as
all miners follow their own economic incentives {% cite nakamoto %}, a property
called incentive compatibility. Thus, compliance by the miners in
Bitcoin is a Nash Equilibrium and this would imply incentive
compatibility for Bitcoin as no miner would have any incentive to
unilaterally change strategy. This would imply a notion of weak
stability if other equilibria exist and strong stability if universal
compliance were the sole equilibrium. It can be shown that with a
majority of miners behaving compliantly, a single longest (correct)
chain will rapidly emerge implying convergence, consensus and liveness .
The paper also quotes various attacks explored by researchers such as
*Selfish Mining*, *Goldfinger Attack*, *Feather Forking* etc.

### Alternative Consensus Protocols

Bitcoin's consensus protocol has raised many concerns about it's
performance, scalability and wastage of computational resources.
<!-- [//]: # (Discussion on performance has been discussed in detail in the post on Speed-Security Tradeoff) -->

#### Alternative Computational Puzzles

There has been ongoing research in trying to replace Bitcoin's
computational puzzle with puzzles which are ASIC Resistant,
Non-outsourceable or which serve other useful purposes outside of the
Bitcoin system (e.g. Permacoin {% cite permacoin%})

#### Virtual Mining and Proof-of-Stake

In Proof-of-Work, getting the right answer is very hard and expensive,
and you get rewarded for getting it but there is a lot of power
consumption. In contrast to this in Proof-of-Stake, getting the right
answer is very easy, but getting the wrong answer is very expensive
because you get punished for getting it and by avoiding the consumption
of computational cycles, no real-world resources are wasted. However, it
is susceptible to the nothing-at-stake problem which is due to costless
simulation attacks since it costs nothing to construct an alternate view
of history in which the allocation of currency evolves differently.

### Anonymity and Privacy

Due to the public nature of the blockchain it is sometimes possible to
trace the flow of money between pseudonyms and conclude that they are
likely controlled by the same individual which could be a threat to the
privacy of the individual. Linking can also be applied transitively to
yield clusters of addresses, this is an instance of transaction graph
analysis.

#### Proposals for Improving Anonymity

The paper compares various techniques proposed to preserve anonymity:
Peer-to-peer mixing protocols, distributed mix networks and privacy
preserving altcoins like Zerocoin.

### Extending Bitcoin's Functionality

Bitcoin's scripting functionality could potentially be used for many
applications other than serving as a digital currency e.g. Colored
Coins, Secure Timestamping, Overlay Protocols etc.

## Conclusion

Bitcoin is a rare case where theory is ahead of practice with many open
questions for the research community on the design of bitcoin, it's efficacy and
the possibility of better alternatives. We still do not have a model which
explains what happens on modifying different parameters or environment of
Bitcoin.

References
----------

{% bibliography --cited %}