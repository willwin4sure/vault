The third step of query processing: formulating the "best" plan (e.g. in terms of time/memory complexity).

This involves two levels of planning:

1. **Logical planning**, i.e. the ordering of operations. This is an exponential search space, but we have various heuristics.
2. **Physical planning**, i.e. the choice of operator implementation and access methods.

## Cost Model

Our goal is to minimize cost. This can be measured in multiple ways:

* Disk I/O (pages read).
* Memory accesses.
* CPU cycles.
* Comparisons.
* Records processed.

Usually we will care just about CPU and I/O costs. CPU costs will be measured in terms of time complexity of the algorithm, while I/O will be measured via
$$
(\text{\# seeks})\cdot(\text{random I/O time}) + (\text{sequential bytes written})\cdot(\text{sequential B/W}).
$$
This is because seeking to arbitrary pages on disk takes nontrivial time; we want to optimize for sequential accesses as much as possible; you know this well from [[🚀Performance Engineering Portal|performance engineering]]. 

## Buffer Pools

In reality, when you pull pages from disk, you will cache them in a ==buffer pool== in main memory. This requires an eviction policy, e.g. LRU or MRU (locality is a bit weird here).

This also allows you to perform all kinds of other optimizations, e.g.

* Multiple buffer pools.
* Pre-fetching.
* Scan sharing.
* Buffer pool bypass.

==Scan sharing== is the synchronization of sequential scans of large tables, so they can take advantage of each others' work.

## Selinger Selectivity

This is what allows us to estimate the cost of query plans in order to optimize them. The basic idea is to just assume every column of each row is drawn *uniformly at random* from the possible values. 

> [!definition] (Cardinalities)
> * `NCARD(R)` is the number of records in `R`, its relational cardinality.
> * `TCARD(R)` is the number of pages `R` occupies.
> * `ICARD(I)` is the number of keys (distinct values) in the index `I`.
> * `NINDX(I)` is the number of pages occupied by `I`.

> [!example] (Selinger selectivities)
> For various predicates, we have the following cases:
> 
> 1. `col = val`. The selectivity is `F = 1 / ICARD` if an index is available, else `1 / 10` (arbitrary).
> 2. `col > val`. The selectivity is `F = (max_key - val) / (max_key - min_key)` if an index is available, else `1 / 3` (also arbitrary).
> 3. `col1 = col2`. The selectivity is `F = 1 / MAX(ICARD(col1), ICARD(col2))` if indices are available, else `1 / 10` (still arbitrary). A better estimate is `1 / ICARD(pk table)` for key-foreign joins.

For more complex predicates involving boolean conditions, we just assume the two are independent and compute the selectivity accordingly.

Using this model, we can estimate the sizes of tables along the path of the join operation. Here is an example from a problem set:

![[Query11.jpg|center|512]]

Usually, a good heuristic is to ensure that intermediate sizes are as small as possible. There is a trivial $n\cdot 2^{n}$ DP algorithm to compute the minimal cost.

> [!warning]
> Selinger's model only considers left-deep trees, i.e. ones where the right side of a join is never the result of any join operation.

These size estimates also allow us to choose the appropriate [[Query Execution#Joins|join operators]] to implement physically.

---

**Next:** [[Query Execution]]