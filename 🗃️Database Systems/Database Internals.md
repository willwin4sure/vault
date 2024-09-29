

You plug a SQL query

```sql
SELECT *
FROM animals
WHERE species = "Giraffe"
```

and you need to get out a table at the end. You can even have parameterized queries using question marks:

```sql
SELECT name, addr, balance
FROM accounts
WHERE userId = ?
```

Database systems can support:

* Interactive queries.
* Web applications.
* Large-scale science.

![[database_internals.png|center|512]]

We are mainly interested in the relational query processor and the transactional storage manager.

## Query System

Takes in SQL.

Parser parses into a parse tree.
Rewrite converts into a different equivalent query.
Planner writes a query plan that is optimized.
Executor interacts with the storage system to get the data.
