---
title: [Ethereum White Paper](https://www.researchhub.com/paper/819634)
link: https://www.researchhub.com/paper/819634
author: Edward Chen
date: 9/15/22
tags: Crypto
---

# Day 7 of Summarizing a Paper a Day: Ethereum White Paper (9/15/22)

**I do want to note that some stuff here is most likely wrong as I'm not a crypto expert in the slightest. Please email me at echen99[at]gmail.com if something is incorrect.**

Launched in 2015 by Vitalik Buterin, Ethereum is now one of the biggest cryptos on the market. Before we dive into Ethereum, let’s analyze Bitcoin quickly.

**Note: Sorry, I'm reading this and the explanation is really bad here. I wrote up a Bitcoin paper earlier, but can't find it, so instead I'll recommend the [Bitcoin white paper](https://bitcoin.org/bitcoin.pdf) for intuition.** Suppose we view Bitcoin as a state transition system; initially, the states are ledgers of all account balances for wallets and transitions as changes between these states e.g. sending money. We can maintain the state pretty easily by tracking the current state on every node in the network encoded in the blockchain. With transactions though, because it is decentralized, there is no "central" node keep tracking of the central state. Instead, we need to be careful in tackling the double spending problem, where an attacker could make 2 transactions: one transferring money to IRL currency and one to another wallet. We try to protect from this possibility of hiding the former transaction with the latter using a consensus system, where individual nodes on the network “vote” with computing power (previously) to determine which transactions are valid. As such, if the network of nodes uses the longest chain as the "correct" one, an attacker would have to rewrite previous blocks on the chain by outpacing the rest of the network(nearly impossible).

Now, what do each of these individual nodes actually look like, and how do we organize them? Vitalik proposes a Merkle tree, a binary tree (at most 2 children) that uses a large amount of leaf nodes to propagate hashes and information upward with intermediate nodes. Hashes are computed for intermediate nodes using the hash of their children, making it such that the network is decentralized while also rendering malicious nodes unable to propagate if they do not have the correct hash.

![hi](img/09_16_Merkle.png)

With this background, let’s look at Ethereum. Ethereum exists on the ethereum blockchain instead of the Bitcoin chain and one of its separating factors is its ability to initiate contracts, like autonomous agents that carry out the code set forth in the contract when activated by a trigger set in the contract. Each Ethereum account carries a contract code, if present, as well as a nonce, ether balance, and storage.

Additionally, for each contract and transaction, miners are incentivized to run the required computation with gas fees, similar to Bitcoin. This is crucial to preventing malicious attacks as the computational power necessary would be very expensive and to preventing infinite loops with unnecessary computations. Gas must be paid upfront and contracts are run until completion or gas runs out, of which gas is only returned if there remains any when the contract is run till completion.

These Ethereum contracts are built into each transaction, and thus are run by nodes validating the Ethereum blockchain. Code is written in a low-level, stack-based bytecode language, called “Ethereum Virtual Machine” or “EVM.”

## Applications

I think the coolest part of Vitalik’s paper lies in the application of using these contracts for financial derivatives and stable-value currencies. A lot of people’s initial hesitation in investing in crypto, including mine, is the volatility of these currencies. Smart contracts solve this by enforcing transactions that allow for the existence of stable coins, such as USDC and USDT, which are pegged to the value of USD with a reserve pool. They also allow derivatives and new financial instruments (e.g. perpetuals) to be traded.

Other amazingly cool applications that made me recently think crypto isn't all BS is its applications in Decentralized File Storage and Autonomous Organizations. Decentralized File Storage could be used to let users rent out their hardware and storage to other users on the internet, where this solution is more fitting for users who have a lot of data, but not enough for enterprise solutions such as Dropbox. Vitalik also correctly predicted the emergence of Decentralized Autonomous Organizations (DAO’s) in 2015, now commonplace in crypto projects today such as Uniswap’s UNI token.

In the end, reading his paper made me a little less adverse to the crypto space.
