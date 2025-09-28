> Associated paper: [Raft](https://pdos.csail.mit.edu/6.824/papers/raft-extended.pdf).

A bunch of systems have a single point of failure, e.g.

* The coordinator in [[🚣Distributed Systems/MapReduce|MapReduce]].
* The master in [[Google File System (GFS)]].
* The storage server in [[Primary-Backup Replication|VM-FT]].

This was to avoid any sort of split-brain syndrome. Raft can allow for higher availability by replacing these single points of failure with distributed systems.

## Quorum Protocols

> [!warning]
> One key issue in replication is that it is impossible to distinguish failure from network partitions, so it is unclear whether you should proceed.

The key idea in Raft is the majority rule.

> [!idea] (Majority rule)
> A client operation succeeds if you manage to update a majority of the servers.

If you always talk to a strict majority, then there is always an overlap for any two consecutive operations. However, since you only need a bit more than half the servers, you can still proceed in spite of network partitions.

A couple systems in the 1990s used these types of protocols: Paxos and view-stamped replication. Raft is also a quorum protocol.

## Raft Overview

Raft centers around log replication. Suppose a K/V server wants to use the Raft library to achieve fault tolerance. Clients can give incoming get/put requests, and the server will take these operations and hand them off to the Raft library to replicate the log across multiple servers. Only when operations are committed are they returned to the K/V server to be installed.



---

**Next:** [[Google File System (GFS)]]