#MIT #CS #systems #distributed

From the MIT course [6.5840 Distributed Systems](https://student.mit.edu/catalog/search.cgi?search=6.5840) taught by Frans Kaashoek and Robert Morris.

A ==distributed system== consists of multiple networked cooperating computers.

> [!question]
> Why distributed systems?

* Performance (too much data to process).
* Fault tolerance (machine failures are alright).
* Inherent (e.g. IoT, lots of devices across the world).
* Security (split into pieces).

One important problem in large distributed systems is consistency: we want a good model of how state is preserved across machines. You've seen discussion about this in [[Eventual Consistency and Amazon Dynamo|database]] lectures.

Another problem is scalability. For example, as you add more computers, your performance is often bottlenecked by the slowest machine, e.g. when you don't load balance correctly or when you must consult many machines through network requests for the necessary data.

In particular, we will see a lot of tradeoffs between these design goals.

1. [[🚣Distributed Systems/MapReduce|MapReduce]]
2. [[RPC and Threads]]
3. [[Primary-Backup Replication]]
4. [[Linearizability]]
5. [[Raft]]
6. [[Google File System (GFS)]]
7. [[ZooKeeper]]
8. [[🚣Distributed Systems/Distributed Transactions|Distributed Transactions]]
9. [[Spanner]]
10. 
