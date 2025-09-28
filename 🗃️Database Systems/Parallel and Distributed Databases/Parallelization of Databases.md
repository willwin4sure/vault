We define ==speedup== as old time over new time on same problem, with $N$ times more hardware. We define ==scaleup== as old time over new time with $N$ times more hardware *and* $N$ times larger problem size.

For databases in particular, we are interested in running transactions and processing queries. You've seen in [[DAGs, Work, and Span|performance]] concepts like Amdahl's law and sublinear, linear, and superlinear speedup. 

Luckily, query processing in the [[The Relational Data Model and Relational Algebra|relational data model]] is embarrassingly parallel.


## Parallelization Strategies
### Shared Disk Parallelism

Popularized by Oracle, reborn a bit in the cloud era. We have multiple machines with their own processors and main memory, and they share a single disk as a [[Multicore Programming#^029c51|persistent medium]] for communication. 

However, this requires coordination of use of the disk and a reliable disk array for fault tolerance.

### Shared Nothing Parallelism

This idea is to have separate disks as well. The machines are simply connected with a fast interconnect (Ethernet, Infiniband), and the data is partitioned across them. This scales very well and allows for fault tolerance via replication.

### Shared Nothing on Distributed File System

One of the leading current approaches, where you share nothing but work on a distributed file system (e.g. HDFS, S3), which is replicated on its own.

> [!idea]
> This decouples the scaling of storage from the scaling of compute!

The storage layer implements its own fault tolerance. Logically, data is still partitioned and operated on by different processors.

This architecture is dominated cloud computing.

## Parallel Query Processing

Three main ways to parallelize:

1. Run multiple queries, each on a different thread.
2. Run operations in different threads ("pipelines").
3. Partition data, and process each partition in a different processor.

We will focus on the last one, which is the holy grail. There are a few partitioning strategies:

1. Random / round robin. This makes all partitions equal size but we have no structure to exploit, e.g. for filtering out partitions via predicates.
2. Range partitioning. Allows potential filtering of partitions but subject to skew.
3. Hash partitioning.

### Join Strategies

If tables are partitioned on the same attribute as the join, can just do local joins. Otherwise, we have several strategies:

1. Collect all tables at one node. Really defeats the purpose of parallel processing, unless we have very small tables.
2. Re-partition one or both tables, a so-called ==shuffle join==.
3. Replicate the smaller table on all nodes.

> [!example] (Pre-partitioning)
> Tables `A` and `B` are both pre-partitioned according to hash function `F` on properties `A.a` and `B.b`, and then we join on `A.a = B.b`. Then, each node can run independently and we can merge the results at the end.

> [!example] (Re-partitioning)
> Table `A` is pre-partitioned on `A.a` but `B` is pre-partitioned in a different way. Then, we can repartition `B` by shuffling the data across the nodes. If there are `n` nodes, this requires each machine to send `(|B| / n) * (n - 1) / n` bytes in total.

> [!example] (Replication)
> If table `B` is small, maybe we just replicate it across all the nodes. Now, each machine needs to send `(|B| / n) * (n - 1)` bytes in total.

> [!question]
> Suppose `|A| = 100` and `|B| = 1`, with `n = 3`, where `B` is pre-partitioned on the join attribute. Would you prefer to re-partition `A` or distribute `B`?

Another technique is called ==semi-joins==, where you send a list of join attributes in each partition of `B` to `A`, and then send a list of matching tuples from `A` to `B`, before computing the join at `B`. This is good for selective joins of wide tables.

---

**Next:**  [[🗃️Database Systems/Parallel and Distributed Databases/Distributed Transactions]]