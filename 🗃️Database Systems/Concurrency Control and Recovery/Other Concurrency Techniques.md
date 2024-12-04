## Other Isolation Levels

There are some other levels of guarantees rather than [[Transactions and Serializability#Serializability|serializability]], e.g. Oracle offers READ COMMITTED by default:

> [!definition] (Other isolation levels)
> None of these guarantee serializability.
> 
> * ==READ UNCOMMITTED== means it is okay for transactions to read others' dirty data.
> * ==READ COMMITTED== means transactions can only read committed values.
> * ==REPEATABLE READS== means transactions will only read the same thing if read repeatedly, i.e. there is no chance of someone swooping in.

In terms of locks:

* For READ UNCOMMITTED, you don't even take a shared lock during reads; you just read the value even if it could be uncommitted.
* For READ COMMITTED, you do take a shared lock but you immediately release it.
* For REPEATABLE READS, you have to keep the shared lock around.

> [!warning]
> REPEATABLE READ is essentially the same as serializability, except you don't have phantom protection. 

> [!definition]
> A ==phantom== is when you are only locking on the granularity of records and then someone inserts a new record into a range that you are considering; in this case, your range sums might be different if the insertion happens in between.

This can be prevented with range locks on ranges in the B+ tree.

## Snapshot Isolation

==Snapshot isolation==. Whenever you start a transaction, take a snapshot of the database and do everything with respect to this snapshot. Then, when you commit, look for write-write conflicts with any other transaction, in which case you abort.

This is better for read-heavy workloads where you won't have starvation. 2PL is better if it's write-heavy.

Note that you can have ==write-skew==, as a result of the fact that you only check for write-write conflicts. Indeed, consider the case of two transactions that check each other for a condition, and then set themselves (e.g. a doctor can only go on vacation if the other is present); then, you could accidentally have both go on vacation if the transactions run at the wrong time.

---

**Return:** [[⛺Concurrency Control and Recovery Homepage]]