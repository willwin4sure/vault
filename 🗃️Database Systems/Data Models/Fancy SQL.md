> Correlated subqueries, recursion, self-joins.

## Correlated Subqueries

A ==correlated subquery== is a subquery that is executed once for each row of the outer query. This is different from a ==nested subquery==, where the inner request just runs a single time.

## Aliases and Ambiguity

You can have ==ambiguity== if two tables have the same column name; one fix is just qualifying it with the table name. Or you can give the table name an ==alias== and use that to qualify instead. These aliases are what you end up using in ==self-joins==.

## Left Joins

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

## Common Table Expressions

> [!idea]
> Anywhere you use a table, you can use a query instead! (Queries just gives tables.)

This is called a ==common table expression==.

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

## Recursive SQL

You can write ==recursive queries== in SQL. These can be used, e.g. to perform a graph search. Here's a basic example.

```sql
WITH RECURSIVE t(n) AS  %% t has a single column with name n. %%
(
	SELECT 1 as n  %% Base case. %%
UNION
	SELECT n + 1  %% Recursive step. %%
	FROM t WHERE n < 100
)
SELECT sum(n) FROM t;
```

> [!idea]
> Now SQL is Turing complete. You could make a Sudoku solver in SQL. You shouldn't make a Sudoku solver in SQL.

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

## Window Functions

==Window functions== allow you to compute things like counts, cumulative sums, and lagged quantities. For example:

```sql
SELECT hour, min, cume_dist()
OVER (ORDER BY hour, min) as c FROM times
```

```sql
SELECT hour, min, qty, lag(qty, 1)
OVER (ORDER BY hour, min) as lag FROM times
```

In particular, lag and cumulative window functions have to be performed `OVER` some `ORDER BY`. Here's another example:

```sql
SELECT day, (sales - (lag(sales, 7)
OVER (ORDER BY day)) as sales_difference)
FROM sales_table
```

---

**Next:** [[Entity Relationship Diagrams]]