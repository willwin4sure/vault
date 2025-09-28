> Provides an interface for larger atomic actions.

Some of this you saw in [[🖥️Computer Systems Engineering Portal|6.1800]].

> [!definition] (Transactions)
> A ==transaction== is a sequence of related actions which are "all or nothing".
> * If the system crashes, partial effects are not seen.
> * Other transactions should not see partial effects.

> [!example]
> One trivial way to ensure correctness is to serialize all transactions.

From a performance perspective, this is terrible. We want things to be able to run in parallel!

## ACID Properties

^8dd786

> [!definition] (ACID)
> The ==ACID== properties of transactions are:
> 
> 1. **A**tomicity: many actions look like one atomic unit; "all or nothing".
> 2. **C**onsistency: database preserves logical invariants.
> 3. **I**solation: concurrent actions don't see each others' results.
> 4. **D**urability: completed actions are in effect after a crash; "recoverable".

The reason we like transactions is because it makes concurrent programming dramatically easier for the programmer! It guarantees that concurrent actions are equivalent to some serial execution!

## SQL Transaction Syntax

1. BEGIN TRANSACTION: followed by SQL operations that modify the database.
2. COMMIT: makes the effects of the transaction durable.
	* After the commit returns, even if the database crashes it guarantees the update.
	* Results will be visible to other transactions.
3. ABORT: roll back the effects of the current transaction.

## Serializability

Suppose that each transaction consists of a sequence of reads/writes that are truly atomic, and we want to schedule them in some order.

> [!definition] (View serializable)
> An ordering of instructions in schedule `S` is ==view serializable== if and only if there exists a serial schedule `S'` such that
> 
> * Every value read in `S` is the same as the value that was read by the same read in `S'`.
> * The final write of every object is done by the same transaction `T` in `S` and `S'`.

Essentially, this is saying that every "view" of the database contents by any transaction (through reads) and at the very end (through the final writes) is consistent with some serial order.

> [!definition] (Conflict serializable)
> Two operations conflict if they are on the same object and at least one is a write. A schedule is ==conflict serializable== if it is possible to repeatedly swap non-conflicting operations to derive a serial schedule.

> [!claim]
> All conflict serializable schedules are view serializable.

> [!idea]
> View serializability is *what you want*, but it is prohibitively expensive to check for. Instead, we enforce conflict serializability, which does guarantee what we want.

> [!example] (Some schedules are view but not conflict serializable)
> This happens when you have transactions with ==blind writes==, where they write a value without reading it.

## Two-Phase Locking

This is a protocol for enforcing conflict serializability. Before acting on an object, a transaction has to acquire a ==shared lock== for reads and an ==exclusive lock== for writes. If a transaction can't acquire a lock, it waits.

> [!example] (Two-phase locking)
> In two-phase locking, for any given transaction, it cannot release *any* of its locks until it has acquired *all* of its locks. This prevents other transactions from "sneaking in" and modifying things during its operation.

> [!idea]
> Two-phase locking has a *growing phase* and a *shrinking phase*.

Then, the serially equivalent schedule can be deduced by ordering by ==lock points==, i.e. places where each transaction acquires their last lock.

> [!warning]
> The main issue with two-phase locking is ==deadlocking==, where two transactions wait on each other indefinitely.

One way to resolve this is via a ==wait graph==, where you DFS to look for cycles and abort a transaction to resolve.

> [!warning]
> Another issue is called ==cascading aborts==, which comes about because we allow transactions to start releasing locks after their lock point but before their commit. In this case, transaction `T1` can write some data that is read by transaction `T2` before `T1` aborts, in which case `T2` is forced to abort as well.

We have some stricter protocols that prevent this from happening:

> [!example] (Strict and rigorous two-phase locking)
> * In *strict* two-phase locking, you always hold *exclusive* locks until end of transaction.
> * In *rigorous* two-phase locking, you always hold *all* locks until end of transaction.

^27d56b

Strict is sufficient for preventing cascading aborts since nobody can sneak in and use something you just wrote to (but released the lock of).

However, in practice, often you don't know when you can even let go of shared locks, since you need to ensure that you never use that object again (maybe if a user is streaming in commands through a terminal, you can't know this). In this case, it will just implement the rigorous version.

---

**Next:** [[Locking Granularity]]