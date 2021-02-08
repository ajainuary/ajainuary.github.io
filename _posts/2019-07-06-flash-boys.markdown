---
layout: post
title:  Paper Review - Flash Boys 2.0
date:   2019-07-06 19:30:35 +0100
image:  11.jpg
tags:   paper-review blockchain research economics
---

I had the pleasure of attending a talk by [Prof. Ari
Juels](https://www.arijuels.com/) at [EPFL's IC Research Day
(2019)](https://archiveweb.epfl.ch/icresearchday2019.epfl.ch/) on Frontrunning
in Decentralised Exchanges (DEXes). Interested readers can watch the talk
[here](https://portal.klewel.com/watch/webcast/epfl-ic-research-day-2019/talk/LGMQpYNsqi6QbcxPLFuWYD/)
and read more about the frontrunning in DEXes on the [post on Hacking,
Distributed](http://hackingdistributed.com/2017/08/13/cost-of-decent/). In this
post, I present a brief overview of the paper by {% cite flashboys %}.

Frontrunning is defined as the act of making a trade based on
information on trading activity before it is available to the
public[^1]. This information might be of an order which is yet to be
executed and might affect the price of the asset. This act is illegal
and makes the market unfair, hence it is strictly monitored by
regulatory bodies in centralized exchanges. 

### Frontrunning in Blockchain

In a decentralized environment, it is not possible to order the
transactions in FIFO (First-In-First-Out) manner since different nodes
might receive the transaction in different orders depending on the
network delay.

Generally, the miners chose to order the transactions in a blockchain
that benefits them the most. In Ethereum, miners prioritize transactions
based on their gas price. Thus, a user can front run another transaction
by sending his own transaction with a higher gas price.

### Frontrunning in Decentralised Exchanges

#### Continuous-Limit Exchanges

Generally, both centralized stock and commodity exchanges and
decentralized cryptocurrency exchanges share a common exchange design
element known as a *continuous-limit order book*. Such an order book
consists of a combined list of all pending offers from buyers and
sellers in the system and operates in the following manner:

-   Buyers place an order specifying the maximum price at which they
    will purchase an asset

-   Sellers place an order specifying the minimum price at which they
    will sell an asset

-   The exchange will then match orders to complete a transaction.

-   The above steps happen continuously in a FIFO manner (hence,
    *continuous-limit*).

Decentralized Exchanges manage the order book via smart contracts.
Typically the order books will be maintained off chain.

#### Insertion Attack

If a bot can observe a pending transaction for purchase with a maximum
price higher than the minimum price of another pending sell transaction.
It could front-run the purchase order and insert two orders to buy at
the lower price from the pending sell transaction and another one to
sell the same asset at the maximum price of the pending sell order.
Thus, the bot makes a profit off the spread.

#### Pure Revenue

Since, these orders through smart-contracts can execute batches of
orders sequentially atomically, reverting back in case any single
transaction fails. This, guarantees a revenue to the bot without any
chance of making a loss due to failure of a single transaction.

#### Competition among bots

There might be multiple bots competing for the an opportunity to
front-run. This ensues a race among the bots to place their transactions
first, the rest will lose out on the opportunity. The race takes place
in the form of bidding by offering higher reward to the miners (*Miner
Extractable Value*), this is termed as *priority gas auction* or PGA.

Priority Gas Auctions
---------------------

PGA is modelled as a sequential game among a set of $n$ players who
compete against each other to obtain a payoff of \$1. The model has
the following main characteristics:

-   **Continuous Time** Since blockchain networks are asynchronous, the
    paper models players actions in continuous time.

-   **Imperfect Information** Players eventually see one anothers bids,
    but not immediately in order to model network latency. Player $P_i$
    will receive bids after $\Delta_i$ and players having smaller
    latencies will have a competitive advantage.

    *Note:* $\Delta_i$ is measured with respect to miners. Thus
    $\Delta_i = 0$ indicates $P_i$ has the same delay as miners.

-   **All Pay Auction** All players will need to pay gas costs for
    transactions even if they fail.

### Auction Strategies

The paper presents and examines 2 non-cooperative strategies for bots.

#### *Reactive* Counterbidding

The player bids s initially (which is the minimum starting bid,
which should be greater than transaction it is trying to frontrun), if
it then observes another bid $b' > b$ then it immediately counterbids
$\min(\max(b\times(1+\delta), b' + \epsilon), 1 + l(b))$ where $\delta$ is the
smallest possible increment on the blockchain (set at 12.5% in Ethereum).

#### Blind Raising

The player keeps on raising his own bids under a predetermined schedule
regardless of other player's bids observed. This can yield an advantage
of not waiting for the other player's bid to arrive.

#### *Comparison*

Counterbidding can achieve an advantage over blind raising only if
$\Delta_i$ is small with respect to $\delta$ (minimum waiting time due
to throttling in the peer-to-peer network). If $\Delta_i > \delta$ there
will exist a blind raising strategy that yields an advantage over
counterbidding.

Security Threat due to Frontrunning
-----------------------------------

The paper claims that frontrunning at the application layer poses a
threat to the underlying consensus layer. The ability of a miner to reap
rewards through reordering user's transaction and potentially inserting
their own transactions creates possibility of attack on the blockchain
and despite this the miner could also collect transaction fees from PGA!

#### Undercutting Attack

When the block reward is dominated by transaction fees, it could have a
high variance. The miner could fork a high-reward block and collect only
some of the fees, leaving the remainder to incentivize other miners to
mine on top of this block. If successful, it would reduce the security
provided by block confirmations {% cite carlsten2016instability %}.

#### Time-bandit Attack

The miner could fork the blockchain in an alternative chain in which he
collects all the rewards and also wins all the auctions (like if he had
a time machine). Using the profit from the collected rewards, he could
pay for the computation cost.

Proposed Solutions
------------------

### Using Trusted Hardware {% cite patent_tee_exchange %}

Using Trusted Execution Environments (TEEs), one could encrypt the
transactions in an isolated environment in which they cannot be
observed. This solution is not perfect due to vulnerability to
side-channel attacks since if one could guess that a frontrunnable
transaction is going to come from a particular IP address, he/she could
still frontrun it.

### Submarine Sends {% cite libsubmarine %}

Submarine Sends is a method using commitments using which transactions
are concealed among other transactions which appear similar to this one.
This is implemented using an intermediate smart contract. Later when the
commitment is revealed, it is not possible to frontrun the transaction
since the exchange would not be accepting commitments any more.

### Batch Auctions {% cite batch_auction %}

Batch Auctions have been a proposed mechanism in traditional centralized
exchanges to solve Frontrunning.

References
----------

{% bibliography --cited %}

Footnotes
---------

[^1]: The term originates from the era when stock market trades were
    executed via paper carried by hand between trading desks. The
    routine business of hand-carrying client orders between desks would
    normally proceed at a walking pace, but a broker could literally run
    in front of the walking traffic to reach the desk and execute his
    order first.