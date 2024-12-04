The fourth step of query processing: actually executing the query plan.

There are a few general types of operations:

1. **Scan** from disk or memory.
2. **Filter** records.
3. **Join** records.
4. **Aggregate** records.

This requires thinking about the physical layout of the data. For now, we will consider tables stored in row-major order; this makes things like insertion easy. We will challenge this notion when we discuss [[Column Stores]].

## Scans

There are two main types of scans: heap scans (physically go through all the pages; note that DBMSs use their own paging systems) and index scans (e.g. using a hash or B+ tree data structure). Such an index data structure must be stored separately (on its own pages), and may be unclustered or clustered.

Other considerations:

* Extensible hashing, where you can easily create more buckets if you fill up.
* Multidimensional data (quad trees? R-trees?).

Here are algorithms for indexes (including scan complexities):

![[index_algos.png|center|512]]

## Joins

For this section, for any table $S$, we will write $\{ S \}$ for the number of records in $S$ and $|S|$ for the number of pages in $S$. First up, we have the nested loop joins:

* **Nested loop join**. This is really stupid, since if the inner table $R$ doesn't fit in memory, you will load it all in $\{ S \}$ times.
* **Block nested loop join**. Solves the previous problem by loading a block of $S$ at a time and comparing it to your scan of $R$.
* **Index nested loop join**. Now you just index the bigger one, so it only takes $\{ R \}\cdot D$ time, where $D$ is the depth of the index on $S$.

Then, we have the hash joins:

* **Simple hash join**. This is only really preferable if both tables fit in memory. If the hash function maps to $P$ partitions, then for each $i$ we can through the whole table and break off all the things that hash to $i$ and the rest that hash to $>i$, then compare each partition in memory. Unfortunately, the I/O complexity is then $P(|R|+|S|)$.
* **Grace hash join**. This one is just the best of the hash joins, and only takes $3(|R|+|S|)$ I/O cost by just scanning only once and writing everything into the right place at the start (via buffers and overflow files), and then comparing each chunk.

Here's a summary of the algorithms:

![[join_algos.png|center|512]]

Here, $M$ is the number of pages in main memory.

---

**Return:** [[⛺Query Processing Homepage]]