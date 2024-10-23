

> [!example]
> Suppose your database stored a single feeding time per cage, but now you want to upgrade it to have several feeding times per cage.

You used to have a table called `animals`, but it got upgraded to `animals2` (all the feedtimes got moved to a `feedtime` table). We can maintain backwards-compatibility by creating a ==view== `animals`, a query that pretends to be a table.



## Fancy SQL

> Correlated subqueries, recursion, self-joins.

==Correlated subqueries== exist.

Aliases and ambiguity. You can have ambiguity if two tables have the same column name; one fix is just qualifying it with the table name. Or you can give the table name an alias and use that to qualify instead. These aliases are what you end up using in self-joins.

==Left joins== keep all the records on the left side, appending NULLs if nothing matches. Right joins and full outer joins are analogous.

```sql
SELECT no, COUNT(cageno)
FROM cages LEFT JOIN keeps
ON no=cageno
GROUP BY no
```

You must use `cageno` instead of `*` (which would just count the number of records, giving 1 instead of 0)!

> [!example]
> Find all keepers that keep both bears and giraffes.

> [!idea]
> Anywhere you use a table, you can use a query instead! (Queries just gives tables.)

This is called a common table expression (?)

```sql
WITH giraffe_keeper_ids AS (
	SELECT id
	FROM keepers, keeps, animals
	WHERE id = kid AND cageno = acageno AND species = "Giraffe"
)
	SELECT animals.name
	FROM giraffe_keeper_ids, keeps, animals
	WHERE id = kid AND cageno = acageno AND species != "Giraffe"
```

Recursive queries... e.g. to perform a graph search.

```sql
WITH RECURSIVE t(n) AS
(
	SELECT 1 as n  %% base case?? %%
UNION
	SELECT n + 1  %% recursive step?? %%
	FROM t WHERE n < 100
)
SELECT sum(n) FROM t;
```

??????????????????????????????????????????????????????????????????????????????????????????????????????????????????
# ????????

> [!idea]
> Now SQL is Turing complete. You can make a Sudoku solver.


```sql
WITH recursive_sick_keepers(kid) as (
	SELECT kid as sick_id
	FROM keeps k
	JOIN animals a on a.cageno = k.cageno
	WHERE a.name = "Mike"
UNION
	SELECT k2.kid as sick_id
	FROM sick_keepers
	JOIN keeps k1 on k1.kid = sick_id
	JOIN keeps k2 on k2.cageno = k1.cageno
)
```

Window functions... allow you to compute cumulative sums. These also make SQL Turing complete.

```sql
SELECT hour, min, cume_dist()
OVER (ORDER BY hour, min) as c FROM times
```

```sql
SELECT hour, min, qty, lag(qty, 1)
OVER (ORDER BY hour, min) as lag FROM times
```

The lag/cumsums have to be over some order!


```sql
SELECT day, (sales - (lag(sales, 7)
OVER (ORDER BY day)) as sales_difference)
FROM sales_table
```