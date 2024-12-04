> A particular logging and disaster recovery protocol.

Our goal is to implement [[Recovery#Logging Modes|STEAL/NO FORCE]], the highest performance mode of the quadrants. This means that the storage manager may steal some of your changes from memory onto disk, and you don't always force your changes to disk even after a commit.

> [!warning] (Hard problems with recovery)
> * Indexes like B-trees. Logical inserts can create different B-trees. Further, you might crash when updating a multi-page B-tree.
> * Checkpointing. How costly are these? Do we have to block the system while checkpointing.
> * Recovery time.
> * Crashes during recovery.
> * Escrow updates. These are a way of encouraging more parallelism among transactions.

==ARIES== is the gold standard in logging. It works well and is specified in its full glory. It implements NO FORCE/STEAL, has recoverable recovery, uses both physical and logical ("physiological") logging, has low-overhead checkpoints, and handles escrow updates.

## ARIES Approach

There are two forward passes and then a backward pass:

1. **Analysis** (FW): to see what needs to be done.
2. **Redo** (FW): to ensure DB reflects updates in the logs but not in the tables
	* This includes transactions that will eventually be rolled back!
	* Why? It ensures an "action consistent" state, which allows logical undo.
	* In some sense, you are just "repeating history".
3. **Undo** (BW): to roll back losers.

## Log Record Format

A log record consists of:

* A ==log sequence number==, or LSN.
* The transaction ID that produced this log record.
* The previous LSN written by this transaction.
* A logical undo image.
* A physical redo image.

Further, note that every disk page will have a header with the latest LSN to have updated it.

## Physiological Logging

REDO is described at a physical level, while UNDO is described at a logical level.

> [!idea] (REDO must be physical)
> This is because the database might not be in an actionable state. `INSERT X INTO Y` may involve multiple non-atomic physical operations, like updating an index. You won't know what exactly you need to do.

Instead, we physically describe it as changing bytes on pages.

> [!idea] (UNDO must be logical)
> This is because the physical operation might correspond to a different action on the bytes. For example, suppose you `INSERT A` then `INSERT B` into some index, which then rebalances itself. If you want to undo `INSERT A`, you can't just erase the page you initially wrote into, since it might have shuffled around.

Instead, we logically describe it as an operation. Note that this is not needed in REDO since we just "repeat history" and replay everything.

## ARIES Normal Operation

Two key data structures:

* **Transaction table:** list of active transactions. Holds their transaction IDs and last LSNs.
* **Dirty page table:** list of pages modified but not yet written to disk. Holds page numbers and the LSN that *first* dirtied the page. On flush, we will remove it from the dirty page number.

> [!idea]
> Once we have both of these, the state of the transaction table tells us what we need to roll back (they never committed), while the state of the dirty page table tells us what we need to flush to disk.

> [!warning]
> During recovery, we could end up with flushed pages in our dirty page table, but that's fine since we just recover. The opposite would be catastrophic though; if a page hasn't been flushed we need to know it is dirty.

During normal operation, these data structures will update. Note that:

* Pages are asynchronously flushed to disk. However, the log always must be forced to disk before flushing pages, even though flushes are not logged.
* Further, the log must be forced before we return a ack a `COMMIT` to the user.

Finally, note that checkpoints are special logs that contain the state of the transaction and dirty page tables.

Here is an example of some transactions and the generated log:

![[aries_example_log.png|center|512]]

How about how the data structures update over time? Well, here's what happens up to and including the checkpoint:

![[aries_example_ds_1.png|center|512]]

Now, here's what happens up to and including the flush:

![[aries_example_ds_2.png|center|512]]

Finally, here's what the state looks like when we crash:

![[aries_example_ds_3.png|center|512]]

## Analysis Phase

Start by going to the most recent checkpoint (usually we will keep a pointer to it). So the machine has rebooted and we see the following log file:

![[aries_analysis.png|center|512]]

This allows us to load in the transaction and dirty page tables at this point. We can do this by just replaying the log.

## Redo Phase

This shouldn't start at the checkpoint (which is only for the ARIES data structures), but rather at the minimum recLSN number, i.e. the first LSN that can dirty one of the unflushed pages. We could REDO everything, but can optimize to REDO everything *except if*:

* The page is not in the dirty page table (it already got flushed).
* The LSN is less than the recLSN of the page (it already got flushed, just the page was re-dirtied).
* The LSN is at most the pageLSN (some later operation flushed it to disk after the checkpoint).

After the REDO phase, the state should be identical to pre-crash.

## Undo Phase

Now use the transaction table to undo losers by following LSNs backwards. 

> [!idea] (Truncating log)
> The minimum LSN you ever have to look at is the minimum of the last checkpoint and the recLSNs. We can safely truncate anything beforehand.

## Compensating Log Records

This handles the case where you crash while recovering. A CLR record is written after each UNDO in order to avoid repeating UNDO work. This means the next time you try to UNDO you won't do it again:

![[undo_clr.png|center|512]]

When you've truly finished, you can add your own virtual EOT log record. 

## Disaster Recovery

A different problem, if your database gets hit by a meteor. You have to keep a replica. One nice way to keep it up to date is to just ship the log over.

---

**Next:** [[Optimistic Concurrency Control]]

