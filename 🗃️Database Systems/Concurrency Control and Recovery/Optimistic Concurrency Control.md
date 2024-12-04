Most real database management systems use looser concurrency constraints, rather than enforcing [[Transactions and Serializability#^27d56b|rigorous two-phase locking]].

## Core Ideas and Main Benefits

> [!idea]
> The core idea of optimistic concurrency control is that instead of preemptively preventing conflicts, we will check at the end if there is a conflict and potentially abort.

In particular, you will read things and then write into a per-transaction personal buffer, where no other transactions can see it. Then, you validate to check if the transaction conflicts with earlier (concurrent) transactions. You abort any transactions that conflict, and then finally install writes at the end of the transaction.

You never have to wait for locks and can't have deadlocks. However, transactions that conflict may have to be restarted repeatedly, in which case they can starve.

Modern OCC systems offer insane throughput. One nice thing is instead of a global lock table you can just check transaction conflicts pairwise.

## Implementation

OCC divides transaction execution into three phases:

1. **Read:** the transaction executes on the DB and stores the local state.
2. **Validate:** the transaction checks if it can commit based on your entire read and write sets.
3. **Write:** the transaction writes state to the DB.

## Validation

Transaction IDs are assigned at the end of read phase and the start of validate.

When transaction `Tj` completes its read phase, in order for it to be validated, we require that for all `Ti < Tj`, at least one of the following holds:

1. `Ti` completes its write phase before `Tj` starts its read phase.
2. `W(Ti)` does not intersect `R(Tj)`, and `Ti` completes its write phase before `Tj` starts its write phase.
3. `W(Ti)` does not intersect `R(Tj)` or `W(Tj)`, and `Ti` completes its read phase before `Tj` completes its read phase.
4. `W(Ti)` does not intersect `R(Tj)` or `W(Tj)`, and `W(Tj)` does not intersect `R(Ti)`.

Here, `R()` and `W()` denote read and write sets, respectively.

---

**Next:** [[Other Concurrency Techniques]]