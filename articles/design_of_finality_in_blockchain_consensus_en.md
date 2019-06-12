# The Design of Finality in Blockchain Consensus

With the development of the blockchain technology, especially the continuous exploration of new technologies such as low-latency chain and cross-chain, finality has been discussed and studied in more cases. Before we start the discussion of finality design, let us first understand what is finality.

The literal meaning of finality is immutability, and is a more ancient concept than “decentralization” and “consensus”. Since the birth of ledger,  people can discuss and define its finality, which has nothing to do with any technical issues, because ledger at that time is centralized (with a trust intermediary), and like correctness, finality is also the reflection of the will of the intermediary.

Only after the blockchain technology introduces the decentralized ledger, consensus and finality become technical issues, and we may face situations where finality and consensus cannot be achieved at the same time.

## The Finality of Different Consensus Mechanisms

According to the current research, we have evaluated the finality of various consensus mechanisms.

For example, PoW does not have absolute finality, only probabilistic finality; BFT has absolute finality, so does Casper FFG. YeeCo's Tetris consensus also has absolute finality.

Why do different consensus mechanisms have different levels of finality, and what are the factors responsible for that?

## The Principle of Finality

What is the nature of consensus? Consensus is the process by which people who want to achieve consistency successfully achieve that, and consensus mechanism ensure that people who want to achieve consistency can still achieve consistency when there is a communication barrier or someone misleading other people.

There is something very interesting in it, a bit of a word game, which is, "people who want to achieve consistency" appears as a precondition in some consensus mechanisms (first determine who want consistency, then seek the consistency ), while in some other consensus mechanisms, it appears as the result (claim the consensus of certain participators, never promise it is the consensus of all people though).

For example, a company wants to hold a dinner party, and all of the employees should reach a consensus on where to hold it. The first approach is “authorities have the final say”, which means if the leaders of the company could reach a consistency, then the whole company reach a consensus. The second approach is that some employs reach a consensus first, and then announce it in the company, but if some powerful people vote against it, then the consensus will be changed; it would keep going until all of the employees reach a consensus.  

Both approaches can achieve consensus in some degree, but what about the finality? Regarding the first approach, the consensus has absolute finality since it can’t be changed as soon as it’s reached. Regarding the second approach, the consensus has probabilistic finality, since it can be modified theoretically, though with a verify low probability.

The example actually demonstrates the difference between the BFT consensus and the PoW consensus. Put it in technical language: the BFT consensus first assumes there is a stable consensus range, and on this basis, choose a strong consistency but low liveness mechanism, thus achieving absolute finality; the PoW consensus does not assume a stable consensus range, and the consensus participants can enter and exit at any time. On this basis, a mechanism with high liveness and eventual consistency is adopted, but it still doesn’t have absolute finality. Therefore, the consensus range is the key to determine the essential nature of finality.

## The Necessity of Absolute Finality

As the transactions on the blockchain often represent the ownership transfer of properties, the fact that the transaction can be modified would be fatal to these economic activities. Double spend attack happens through taking advantage of PoW not having absolute finality.

Applications associated with the blockchain is operating based on some kind of finality criteria, such as the well-known Bitcoin 6 confirmations and the Ethereum 12 confirmations. However, things do not always work like that. Everyone can recall that when a fork, a hashrate war, or an attack occurred, the wallets and the exchanges suspended the transactions. This indicates the finality of the PoW is evaluated as vulnerable under such circumstances, so the n-confirmed standard will be adjusted to infinite confirmed standard.

Cross-chain and multi-chain sharding arealso forms of applications associated with the blockchain. Their requirements for finality are much higher than off-chain applications:

Blockchain is a system that operates according to programmed procedures and cannot modify the n-confirmed criteria actively as in the case of off-chain applications.
In terms of double spend attack, it is not easy to find a trading target large enough in the off-chain application. There is not much profit in it and the cost is high, so it is difficult to carry out the double spend attack. However, it is easy to find a trading target large enough in the cross-chain, and the attack becomes profitable.

Therefore, from the perspective of application supporting, the blockchain should be responsible and provide finality, rather than letting the applications evaluate the finality degree by themselves. From the perspective of cross-chain supporting, the security of cross-chain is based on finality. In a word, absolute finality is vital and necessary.

## Consensus Participants’ Game under Absolute Finality

Regarding the consistency of the consensus, if the consensus participants can still agree with the “authorities have the final say” principle (for BFT, it is the collection of voters; for PoW, it is the hashrate), then even if the consensus does not meet their own wishes, they will still choose to "obey"; if the consensus participants do not agree with the principle, then there would be a hard fork.

When absolute finality is introduced, let's take a look at what it means for a consensus with strong consistency and a consensus with eventual consistency.

For the consensus with strong consistency, like that of BFT, strong consistency and absolute finality are a whole. The consensus itself can’t build an alternative chain. Usually an alternative chain is built due to some interference. If some voters made a hard fork happen and build an alternative chain, the finality of the old chain is still guaranteed, and as long as the new chain has its own users, these users also recognize the finality of the new chain. If the collectors collude to rebuild the chain through hard forks, the original chain is discarded, and it seems that the applications on the original chain is affected by the modification of the block, but it is not due to the destruction of the finality, it’s simply due to the death of the original chain.

For the consensus with eventual consistency, like that of PoW, the rebuildability of the chain is preserved in order to support the unstable consensus range. If absolute finality is introduced, a new stable consensus range must have been introduced. In order to ensure the finality of a block, the operation of PoW can’t be just following the longest chain principle. Rather, it should follow the principle of "the longest chain containing the finalized block", that is, even if the super hashrate has the ability to rebuild the chain, it should also"obey" the new stable consensus range, and if the super hashrate is unwilling to obey, then it can make  a hard fork, in which casethe finality of the original chain is still guaranteed. The willingness to obey of the the super hashrate is related to the authority of the “new stable consensus range”. If it has little authority, it is likely for the super hashrate not to obey, in which case, the situation would be a complicated one regarding how the applications would choose, stick with the old chain or switch to the hard fork.

From the above analysis we can learn that absolute finality can lead to two extremes. One is to reach a consistency that cannot be modified; the other is that if a consistency can’t be reached, it will be split into two different consistencies that can’t be modified, and the applications have to choose one between two and recognize the finality of the consistency it picked.

Since there are two extreme cases, when we design the finality mechanism, we should try to avoid the case that can lead to hard forks. We will discuss the finality design of the PoW consensus mechanism in the next part.

In summary, from the perspective of applications, absolute finality turns the risk of block being modified into the risk of blockchain fork, which in turn affects the consensus participants’ game. If such impact can further reduces the possibility of blockchain fork, it would be a successful finality mechanism and that would be very valuable for the applications.


## The Finality Design of the PoW Consensus Mechanism

The classic PoW consensus adheres to the adoption of the unstable consensus range and the principle of the longest chain, and retains the possibility that super hashrate is able to rebuild the chain to achieve new consistency. If we still insist on these, it is theoretically impossible to establish absolute finality, so we should choose a reasonable way to break these limits.

The absolute finality design of PoW consensus mechanism should include the following points:

1. The establishment of stable consensus range

    As mentioned above, the essential requirement of absolute finality is a stable consensus range. The question is how to introduce one, whether we should directly transform the unstable consensus range in PoW and combine the two ranges into one, or introduce a new one and make them run side by side?

    How much influence does the stable consensus range have on the decentralization of PoW, and will it weaken the authority of the hashrate so that the PoW actually degenerates into a voting mechanism?

2. The establishment timing of finality

    Should we establish it and consistency at the same time, or at different times? If at different times, how long should finality be delayed? In theory, the shorter the delay is, the more it is needed for the unstable consensus range (hashrate) to obey the stable consensus range. How to balance them then?	

3. Make stable consensus range compatible with the incentive mechanism

    The stable consensus range is generated by some mechanism, such as Staking. As voting on finality determines the direction of the chain, voters can choose to vote on both sides without any loss. This is nothing at stake problem. How to avoid this problem through incentive mechanism?

4. Failure governance mechanism

    The establishment of finality is to obtain a strong consistency at the cost of liveness, then it will certainly encounter a situation where the consensus on finality cannot be achieved. The advantage of this situation is that it reflects the responsible side of the chain. The height of the finalized block no longer grows, and the off-chain activities will be suspended accordingly. The downside is that a governance mechanism is needed to resolve the failure of reaching a consensus on finality.

5. Support multi-chain sharding

    Off-chain applications need safe access to finality information. The regular way is to run a full node, synchronize finality consensus results and verify. In the multi-chain sharding scenarios, usually each sharding runs the full node of its own and light nodes of all other shardings. This brings out a problem, whether a light node can safely synchronize the finality consensus results. It is an important problem that needs to solve in order to achieve multi-chain sharding.
    
    In addition, if a sharding chain is forked, how to make other shardings identify which is the original chain and which is the forked chain, and ensure that they only interacts with one chain unanimously.

YeeCo's CRFG (Conditional Reward Finality Gadgets) proposes a set of solutions for establishing absolute finality on the PoW consensus mechanism, which will be elaborated in the subsequent articles.