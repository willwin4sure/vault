> Structured Query Language (SQL) is the standard for accessing data from relational models.

SQL enjoys benefits such as ==declarative programming==, [[IMS#^8b1c2b|logical data independence]], and [[IMS#^ff98ad|physical data independence]].

Go visit these pages to learn more:

## Basic Examples

Here are some basic examples of SQL queries:

```sql
SELECT field1, ..., fieldN
FROM table1, ...
WHERE condition1, ...
```

```sql
INSERT INTO table1 VALUES (field1, ...)
```

```sql
UPDATE table1 SET field1 = X, ...
WHERE condition1, ...
```

> [!idea]
> In SQL, you logically specify *what you want* (declarative), not *how you get it* (imperative)! The RDBMS does the rest of the work.

> [!example]
> Here is a more concrete example using the [[Tables, Tables, Tables#The Zoo Data Model|the zoo data model]], where you want to find the names of all keepers who keep both students and salamanders:
> 
> ```sql
> SELECT keeper.name
> FROM keeper,
>      cages as c1, cages as c2,
> 	 keeps as k1, keeps as k2,
> 	 animals as a1, animals as a2
> WHERE c1.cageid = k1.cageid AND keeper.keeperid = k1.keeperid
>   AND c2.cageid = k2.cageid AND keeper.keeperid = k2.keeperid
>   AND c1.cageid = a1.cageid AND c2.cageid = a2.cageid
>   AND a1.species = 'student' AND a2.species = 'salamander'
> ```
> 
 > This query logically involves accessing the `keeper`, `cages`, `keeps`, and `animals` tables (possibly multiple times) and taking their full Cartesian product, and then selecting only the rows with the necessary relationship.

The use of joining a table to itself is called a ==self-join==.

## Generic Syntax

Here is a generic SQL query:

```sql
SELECT [DISTINCT] selectExpression
FROM tableExpression
WHERE expression
GROUP BY expression
HAVING expression
ORDER BY order
LIMIT number
```

---

**Next:** [[Fancy SQL]]
