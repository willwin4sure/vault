[[Transactions and Serializability|So far]], we've had an abstract model of "objects" being read, written, and locked. In practice, it could be a single record, a page, a table, or a whole database. What gives?

## Hierarchy

One solution is to have a hierarchy of locks. However, this appears to defeat the purpose: do you have to grab the database level lock every time you want to do anything?

> [!idea] (Intention locks)
> The solution is ==intention locks==, where you grab intention locks on the higher objects and only the real lock on the lowest one.

Now, here is the full lock compatibility table:

![[lock_compatibility.png|center|512]]

---

**Next:** [[Recovery]]