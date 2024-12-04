> Breaking open database management systems.

You plug in a SQL query

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

![[dbms_internals.png|center|512]]

We will go over a few of the components inside a database management system, shown above. In particular, we will focus on the relational query processor, which also involves some aspects of the underlying transactional storage manager.

1. [[Query Parsing]]
2. [[Query Rewriting]]
3. [[Query Optimization]]
4. [[Query Execution]]

Then, we'll discuss another storage format for databases, column stores. These are commonplace among analytical databases (as opposed to transactional ones).

5. [[Column Stores]]

Then, we'll revisit one part of query optimization and refine our approach.

6. [[Advanced Cardinality Estimation]]

---

**Next:** [[⛺Parallel and Distributed Databases Homepage]]