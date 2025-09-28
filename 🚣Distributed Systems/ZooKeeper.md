> Associated paper: [ZooKeeper](https://pdos.csail.mit.edu/6.824/papers/zookeeper.pdf).

This is a widely used system that is high performance for two reasons: it is asynchronous and doesn't require strong consistency. It can be used in generic coordination services, e.g. keeping track of configuration information such as who is the master.

ZooKeeper is a [[Primary-Backup Replication|replicated state machine]]. Clients send instructions (e.g. creating znodes) which are then distributed into logs using ZAB (imagine something like RAFT) to other servers, and then responses are given to the client.

## Performance and Guarantees

Here is the throughput that ZooKeeper can achieve:

![[zookeeper_throughput.png|center|384]]

There are some reasons why we get such performance:

1. Asynchronous requests, so many can be outstanding for any client.
2. Read operations can be processed by any server.

However, ZooKeeper provides the following consistency guarantees:

1. Writes are [[Linearizability|linearizable]], i.e. there is a total order for them that respects "real-time".
2. FIFO client order, i.e. your writes are in order and you read your own writes.

You can still see stale data since you only get a prefix of the log in general (i.e. other clients exist). But you can't see reads from the past, i.e. subsequent reads see a longer prefix of the log.

## Implementation

When the ZK client writes, it sends to the leader. The write will get appended to the log and the leader returns back the `zxid` (i.e. index into the log), which the client stores. Then, when the client wants to read, it can contact any server (the leader or followers). It sends the `zxid` so that this follower waits for its log to be at least that long (maybe the follower is behind). The follower then returns the data. This could be stale if the follower is behind the consensus.

## How to Use ZooKeeper

Even though we don't have linearizability, the guarantees we do get are sufficient to use. Suppose we are using ZooKeeper to manage configurations.

A new leader (in your configuration) spawns, and executes the following code:

```
del("ready")
write f1
write f2
create("ready")
```

Then, a follower executes

```
exists("ready")
read f1
read f2
```

If we assume that `exists` reads the flag set by `create`, then we also know that our reads of `f1` and `f2` also get the correct data! This is because all the writes are in global total order and reads can't go back in time.

However, what if there was a different interleaving where `exists("ready")` and `read f1` execute before the configuration change, and then `read f2` occurs after. Then you might get inconsistent configuration files!

We can solve this by using the ZooKeeper `watch` functionality. If we write `exists("ready", watch=true)`, then we will receive a notification when `del("ready")` occurs. The rule is that notifications must occur before the next write is issued, so we will get it before `write f1`. 

So then the follower can notice that configuration has changed between its reads, and it can restart.

In general, programming is a bit difficult but is feasible with care.

## Example Usage

Suppose you are using [[Primary-Backup Replication|VM-FT]] and you want to support the test-and-set to avoid two primaries. You can support this using the znode API and version numbers, much like [[Synchronization Without Locks|lock-free programming]]. If there's a lot of contention, you will have considerable retry.

---

**Next:** [[🚣Distributed Systems/Distributed Transactions|Distributed Transactions]]