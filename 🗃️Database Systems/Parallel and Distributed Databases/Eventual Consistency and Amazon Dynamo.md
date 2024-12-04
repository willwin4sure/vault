Driving use case: Amazon's "always available" shopping carts. Amazon doesn't want any outages when people are trying to place items into their shopping carts.

Amazon Dynamo was designed for this use case. It:

1. Favors availability over consistency.
2. Is very low latency.
3. Doesn't support complex analytics.
4. Incrementally scalable, can easily add and remove machines from running system.

> [!theorem] (CAP theorem)
> You can have two of the following properties, but not all three:
> 
> 1. Consistency.
> 2. Availability.
> 3. Partition tolerance.

Dynamo spawned the world of noSQL databases. There is no longer a notion of transactions; it's a basic key/value store where you can `get(key)` and `put(key, context, value)` but nothing else. It respects single-key atomicity.

Dynamo maps everything to a ring and uses consistent hashing. The nodes have some consistent address on the ring, as do any pieces of data. Then, each node is responsible for all the data on the clockwise segment from it. The nice thing about this is you don't have to rehash values if you get a new machine (or lose one).

One solution for ==data skew== is to actually map each machine to multiple positions so you don't randomly have situations where it's responsible for too little or too much data.

You can also replicate easily by making each node responsible for the next two intervals along the range (ignoring it if the node repeats). 

The method for guaranteeing consistency is to always require a read or write quorum among the replicas, where $R + W > N$, if $R$ is the read quorum, $W$ is the write quorum, and $N$ is the number of replicas. Once you hit your quorum, you can proceed. This will work since at least one of the machines you read from must have the latest write.

Quorums still favor consistency too heavily. The solution is ==sloppy quorums==, by continuing around the ring if any machine is down. The guys on the right that shouldn't have actually gotten it perform so-called ==hinted handoffs== to the ones that were down.

You can easily have divergence on any key $k$ if you have a network partition, since one side might believe its some value while the other side believes it's another. Can fix with ==vector clocks==, where each node stores a monotonic version counter.

If you see two incompatible clock vectors, consolidate with some strategy (e.g. if they are commutative just sum up and change to max counters).