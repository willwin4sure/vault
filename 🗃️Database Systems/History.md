IMS is a hierarchical model. Data is organized as ==segments==.

Each segment has a particular physical representation. It could be ordered, hashed, unordered, etc.

IMS provides operations to interact with the system.

This finds the cages that Jane keeps. Note the language is stateful, i.e. you have an implicit pointer of where in the hierarchy you are right now.

```IMS/PL1
GetUnique(Keepers, name="Jane")
Until done:
	cageid = GetNextParent(cages).no
	print cageid
```

You have to know too much about how the data is stored in order to query it! This ties the implementation of the query to the schema way too much.


CODASYL Data Model / COBOL ??


## Relational Data Model

All data is represented as tables of records.
Tables are unordered sets with *no duplicates*.
A database is one or more tables.
Each relation has a *schema* that describes the types of the columns/fields.

Tables will have columns that correspond to foreign keys. Specifying this will *ensure* that these keys are always valid. Further, if a foreign key is still pointing to a record, deleting that record will throw a warning.

## Relational Algebra

* **Projection** $\pi$. Selects a subset of columns from a table.
* **Selection** $\sigma$. Selects a subset of rows that satisfy a predicate.
* **Cross product** $\times$. Combines two tables with a full Cartesian product.
* **Join** $\bowtie$. Combines two tables with a predicate.
* **Set operations**.
* **Aggregate operations**.

These are just logical. The actual implementation can be made more efficient. Nothing about queries prescribe anything about how the database actually stores things.

There are also nice identities which can be used for optimization, e.g. predicate push-downs.