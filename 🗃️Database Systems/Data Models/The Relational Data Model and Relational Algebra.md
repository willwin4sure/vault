## Relational Data Model

We've already pretty much seen how this works in the [[Tables, Tables, Tables]] section.

Ted Codd proposed his ==relational model== in 1970. It was driven by the fact that IMS programmers had to constantly maintain their applications when logical or physical changes occurred. His proposal was threefold:

> [!claim] (Relational model proposal)
> 1. Store the data in a simple data structure (tables!).
> 2. Access it through a high-level set-at-a-time data manipulation language.
> 3. No need for a physical storage proposal. Optimize independently!

The simple data structure allows easier support for [[IMS#^8b1c2b|logical data independence]], while the high level language allows a higher degree of [[IMS#^ff98ad|physical data independence]].

In this model:

* All data is represented as tables of records, which are unordered sets with *no duplicates*. These are also called ==relations==, from math.
* A database is one or more tables.
* Each relation has a ==schema== that describes the types of the columns/fields.

Despite its simplicity, this model is also flexible enough to represent basically anything!

## Relational Algebra

> [!definition]
> ==Relational algebra== is a theory that uses algebraic structures for modeling data and defining queries on it. In particular, one or more input relations combine to create an output relation.
> 
> * **Projection** $\pi$. Selects a subset of columns from a table.
> * **Selection** $\sigma$. Selects a subset of rows that satisfy a predicate.
> * **Cross product** $\times$. Combines two tables with a full Cartesian product.
> * **Join** $\bowtie$. Combines two tables by filtering the full Cartesian product with a predicate.
> * **Set operations**. For example, unions allow concatenation of tables.
> * **Aggregate operations**. For example, cumulative sums or averages.

These are purely logical, while the actual implementation can be made more efficient. For example, there are nice identities which can be used for optimization, e.g. ==predicate push-downs==, where we move selection operations before joins.

> [!idea]
> Nothing about queries prescribe anything about how the database actually stores or accesses things; they are purely logical.

---

**Next:** [[SQL]]