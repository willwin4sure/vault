> Associated paper: [Spanner](https://pdos.csail.mit.edu/6.824/papers/spanner.pdf).

Spanner supports wide-area transactions, and can distribute data at a global scale.

* R/W transactions are implemented with [[🚣Distributed Systems/Distributed Transactions|2PL and 2PC]], where the participants are Paxos groups.
* R/O transactions are very fast, using [[Other Concurrency Techniques#Snapshot Isolation|snapshot isolation]] and synchronized clocks (you have to deal with a little error margin on true time).

This is widely-used by Google.

## Discussion

You have three datacenters A, B, and C in the world. You have a shard in datacenter A but you want to replicate it in B and C so you are fault tolerant. These shards form a single Paxos group. Each shard is a different Paxos group.

The majority-rule of Paxos allows us to tolerate faults and slowness. Further, replicas allow clients (e.g. the Gmail backend) to solely communicate with the closest one.

Here are some challenges we tackle:

* Read of the local replica reflects the latest write. Spanner provides external consistency, a stronger property than [[Linearizability|linearizability]]. This says that if a transaction starts after another is committed, it must see the other's writes.
* Transactions can occur across shards (e.g. bank transfer between accounts on different shards). This can be done with 2PC.
* Transactions must be serializable. This can be done with 2PL.

The read/write transactions occur as you think. The locks are stored only in the leaders of the Paxos groups for speed. It is basically the 2PC and 2PL picture but with Paxos groups. This high availability actually means you have to deal with less issues with 2PC since failures are very rare.

To achieve fast read/only transactions (10x the speed) we only read from local shards, have no locks, and no 2PC. The challenge here is maintaining consistency. You can't just read the last committed value, since then you can't guarantee external consistency (you might read the value of X from one committed transaction then the value of Y from the next committed transaction).

The key is snapshot isolation. We assign a timestamp to each transaction. This is the commit point of any R/W transaction and the start of any R/O transaction. Then: execute all transactions in timestamp order, and have each replica store data with timestamps (i.e. persistent data). Then your R/O transactions can just give the old values to maintain serializability.

However, what if your particular replica of the shard hasn't seen the latest write to the Paxos group? This is solved using "safe time". Paxos will send all writes in timestamp order. Before any read, the replica has to wait for a write with larger timestamp (writes come along all the time) to make sure you don't return the wrong value. You also have to wait for any transactions that are prepared but not committed.

In this case, you want your clocks to be in sync. You have a small uncertainty on any true time that you account for.