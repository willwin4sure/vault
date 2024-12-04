When a system crashes, we assume that the RAM is wiped but the disk persists. After a crash, recovery ensures:

* **Atomicity:** partially finished transactions are rolled back.
* **Durability:** committed transactions are on stable storage.

This brings the database back to a correct state, where committed transactions are fully reflected and uncommitted transactions are completely undone.

## Write-ahead Logging

We follow [[Atomicity#^1d0cb6|the golden rule of atomicity]], and use a write-ahead log where we indicate what we plan to do before we actually do it. This log gets flushed to disk so that it remains durable.

The log will allow us to UNDO and REDO our actions, as well as determine which transactions finished.

## Logging Modes

> [!idea] (STEAL mode)
> Sometimes, we may want to write dirty (uncommitted) pages back to the disk.

You really have no choice but to do this if you need to update more things than you have main memory. However, this requires you to implement UNDO for your database.

> [!idea] (NO FORCE mode)
> Sometimes, some committed changes may not have to be written back to disk.

Suppose you have a poorly written application that keeps updating a record and committing it. Then we are flushing it to disk way too many times. However, this requires you to implement REDO for your database.

Note that if you do FORCE instead, you do also technically have to implement UNDO as well, since the database could crash between FORCE and COMMIT.

Next class, we will discuss a STEAL/NO FORCE recovery method.

---

**Next:** [[ARIES]]