> Associated paper: [GFS](https://pdos.csail.mit.edu/6.824/papers/gfs.pdf).

## Storage Systems

Durable storage systems are great building blocks for fault-tolerant systems, since your app can remain stateless while offloading all the persistence work to the storage system.

Why are storage systems hard?

* You want high performance. So you need to shard data across servers.
* But now you have many servers, leading to constant faults.
* So you need a fault tolerant design, meaning you need [[Primary-Backup Replication|replication]].
* But then there are potential inconsistencies!
* This requires a strong consistency protocol, which may lead to lower performance.

This is a fundamental struggle in designing storage systems.

Ideal consistency is to behave as a single machine. But even then there are different consistency models in the face of concurrency, and you can have race conditions in the face of multiple clients!

For example, a simple replication protocol, which **fails**, is to just have clients send the same instructions to two servers. But if you have nondeterminism in both servers as to which client is handled first (maybe they race to update a value), then you can have different results in the servers!

## GFS Overview

This is a case study in performance, replication + FT, and consistency. Even though it is no longer used at Google, it was a successful system that scales to thousands of machines.

Some unusual design choices are that it has a single master (definitely a mistake as things have scaled), and it doesn't have strong consistency guarantees.

Ultimately, the goals are to be:

* Big: handle a large dataset.
* Fast: automatically sharded.
* Global: all apps see the same file system.
* Fault tolerant: automatically handle failures.
## GFS Design

Files are broken into fixed-sized chunks.

![[gfs_architecture.png|center|512]]

The master is in charge of knowing where things are. So the client might want to open a file, in which case it asks for the file name and chunk index, and the master returns the chunk handle and its location.

Then, the client can go to any of the chunk locations (i.e. different chunkservers holding that chunk) and ask for the actual data.

## GFS Master

The master holds the following state:

* A mapping from file name to an array of chunk handles. This is held in memory for fast responses to clients, but must be durable (e.g. in the log).
* A mapping from chunk handles to a version number and list of chunkservers holding that chunk. One of those is a primary, while the rest are secondaries. There is also a lease time for the primary. These don't need to be durable (except for version number) since they can be read from the chunkservers when the master reboots. The version number needs to be durable so it can tell which chunkservers are stale.
* A durable log and checkpoints.

## Reading a File

1. Client sends file name and offset to the master.
2. Master responds with chunk handle and list of chunkservers and the version number.
3. Client caches the list.
4. Client asks closest chunkserver for the data.
5. Chunkserver checks the version number and then sends the data if not stale.

## Writing a File

If there's no primary yet, the master chooses one, increments version number, and grants a lease time to the primary.

![[gfs_write.png|center|384]]

The client forwards the data to the replicas in any order. The primary checks the version number and that its lease is still valid, and then instructs the other replicas to append at a certain offset. Then if everything succeeds it responds to the client.

You error if one of the chunkservers doesn't respond to the primary, and retry. This is the at-least-once model.

To handle duplicates, the library on top of GFS can give everything a unique ID. You can also have checksums to make sure data isn't garbled.

## Consistency

Here is an example where you can get stale data:

The client caches a version number 10 with a list of chunkservers. This was given to it by the master. Then, one of the secondaries is partitioned from the rest of GFS, and another client issues a write. Due to the disconnection, the master issues a new version number 11 to the primary and all connected secondaries, and the data is updated. However, the first client can still contact the disconnected secondary (by chance) and get stale version number 10 data successfully.

---

**Next:** [[ZooKeeper]]






