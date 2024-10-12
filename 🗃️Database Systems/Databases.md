

## Structured Query Language

Some examples:

```sql
SELECT field1, ..., fieldN
FROM table1, ...
WHERE condition1, ...
```

```sql
INSERT INTO table VALUES (field1, ...)
```

```sql
UPDATE table SET field1 = X, ...
WHERE condition1, ...
```

Rather than **imperatively** telling the database what to do, we **declaratively** ask for some specification that the implementation returns us.

When we have a query that involves multiple tables, the most naive implementation would iterate through the entire Cartesian product. However, ==join== clauses in SQL will allow more efficient implementations.

You may also want to ==self-join== a table to itself if answering a question about pairs of elements of the table. SQL also supported ==nested queries==. 

> [!example] (Find keepers who keep both students and salamanders)
> 
> ```sql
> SELECT keeper.name
> FROM keeper, cages as c1, cages as c2,
> 	keeps as k1, keeps as k2, animals as a1, animals as a2
> WHERE c1.cageid = k1.cageid AND keeper.keeperid = k1.keeperid
> AND c2.cageid = k2.cageid AND keeper.keeperid = k2.keeperid
> AND 
> ```

Optimizations allowed by declarative queries:
- Sorting by type (insertions are slower).
- Store things in a hash table or tree.

SQL -> procedural plan -> optimized plan -> compiled program.

==Predicate push-down==: move filtering to lower (earlier) parts in the procedure tree.