---
layout: post
title:  Paper Review - Bitcoin and Red Balloons
date:   2019-10-10 15:01:35 +0530
image:  14.jpg
tags:   paper-review blockchain research economics
---
The paper by {% cite babaioffbitcoin %} demonstrates that the peer-to-peer layer and the gossip
protocol are not always incentive compatible i.e., the participants do
not internalize sufficient value from propagating information to justify
the opportunity costs of doing the same. The authors also propose a
sybil-proof scheme which is able to incentivize information propagation
in Nash Equilibrium. It is also shown that there are no
dominant-strategy mechanisms which can do the same.

The Incentive Problem
---------------------

Bitcoin relies on a peer-to-peer network to verify and authorize all
transactions that are performed with the currency. The spender signs a
transaction and then distributes them to a few peers who are expected to
propagate the transaction further. Coincidentally, these peers also
compete to verify the transaction and thus gain miner's fee as reward.
One can clearly observe that there is a conflict of interest as the
peers have an incentive *not* to propagate information to others thereby
reducing competition.

Proposed Reward Scheme
----------------------

A successful reward scheme has to posses several properties. First, it
has to incentivize *information propagation* and *no duplication*. That
is, it will be in a node's best interest to distribute the transaction
to all its children without duplicating itself, as well as never
duplicating when it authorizes. Second, at the end of the distribution
phase most of the nodes have to be aware of the transaction. Lastly, we
would like to achieve this with small rewards, while minimizing the
number of seeds, to ease the burden of initial distribution.

### $(\beta, \mathcal H)$-*almost-uniform* Scheme

In this scheme, each node in the chain except $v$ (the verifying node
who happens to be at a level $l$) gets a reward of $\beta$, and $v$ gets
a reward of $1+(\mathcal H-l+1)\beta$. The intuition behind giving the
authorizing node a reward of $1+(\mathcal H-l+1)\beta$ is to mimic the
reward that the node would have gotten if it duplicated itself
$\mathcal H-h$ times so that it has no benefit in duplicating itself
before trying to authorize the transaction itself. The total reward
distributed in the scheme comes out to be $1+\mathcal H \beta$.

The number of initial seeds required for a
$(\beta, \mathcal H)$-*almost-uniform* scheme so that only the profiles
of strategies in which every node of depth at most $\mathcal H$ never
duplicates and always propagates information survive in *every* iterated
removal of weakly dominated strategies is at least
$\frac{2}{\beta} + 13$. Consider the following two variants of the
scheme:

-   $\beta = \frac{1}{\mathcal H}$, in this case total reward
    distributed is $2$ but we require at least $2\mathcal H + 13$
    initial seeds.

-   $\beta = 1$, in this case total reward distributed is
    $1+\mathcal H \in O(\mathcal H)$ but we need only $15$ initial
    seeds.

#### Hybrid Scheme

By combining multiple schemes we can obtain a scheme that has *both* a
constant number of seeds and a constant overhead. The scheme works as
follows: we run the $(\frac 1 H,H)$-almost uniform scheme with a set of
$A$ seeds ($|A|=t-7$) and simultaneously run the $(1,1+\log_d H)$-almost
uniform scheme with a set of $B$ seeds ($|B|=7$) for some $t > 14$. The
expected total sum of payments comes out to be $3$ in this case and
$2+\log_d H$ in the worst case.

Impossibility Result for Dominant Strategy Mechanisms
-----------------------------------------------------

Suppose that $H\geq 3$. There is no Sybil-proof reward scheme in which
information propagation and no duplication are dominant strategy for all
nodes at depth $3$ or less.

Related Work
------------

Johnson et al. {% cite johnson_game-theoretic_2014 %} have studied whether and
when participants in the peer-to-peer protocol are incentivized to
engage in network-level denial-of-service attacks against others. They
conclude that mining pools have an incentive to engage in attacks, that
larger pools are better to attack than smaller pools and that larger
pools have a greater incentive than smaller pools to attack at all.
Denial-of-service attacks against pools are regularly observed in the
wild, so this theoretical analysis can be backed up by observations {% cite
vasek_empirical_2014 %}.

References  
----------

{% bibliography --cited %}

Footnotes
---------

[^1]: Not to be confused with computer networking

[^2]: Anecdotally, the first block of Bitcoin included a reference to
    reserve bank's failure