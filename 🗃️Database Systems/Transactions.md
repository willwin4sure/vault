Concurrency control and recovery.
Distributed and parallel query processing.

Some of this you saw in 6.180.

> [!definition] (Transactions)
> A ==transaction== is a sequence of related actions which are "all or nothing".
> * If the system crashes, partial effects are not seen.
> * Other transactions should not see partial effects.

> [!example]
> One trivial way to ensure correctness is to serialize all transactions.

From a performance perspective, this is terrible. We want things to be able to run in parallel!

> [!definition] (ACID)
> The ==ACID== properties of transactions are:
> 
> 1. **A**tomicity: many actions look like one atomic unit; "all or nothing".
> 2. **C**onsistency: database preserves logical invariants.
> 3. **I**solation: concurrent actions don't see each others' results.
> 4. **D**urability: completed actions are in effect after a crash; "recoverable".

The reason we like transactions is because it makes concurrent programming dramatically easier for the programmer! It guarantees that concurrent actions are equivalent to some serial execution!

SQL syntax:

1. BEGIN TRANSACTION: followed by SQL operations that modify the database.
2. COMMIT: makes the effects of the transaction durable.
	* After the commit returns, even if the database crashes it guarantees the update.
	* Results will be visible to other transactions.
3. ABORT: roll back the effects of the current transaction.

