The second step of query processing: simplify queries.

## View Substitution

A ==view== is a virtual table. These are useful primitives that can also be prepopulated before queries actually arrive. These need to actually be substituted in with queries.

## Predicate Transforms

Remove and simplify expressions. Constant simplifications, exploiting of constraints, and removal of redundant expressions.

## Subquery Flattening

Might be able to remove subqueries entirely. This is not always possible.

---

**Next:** [[Query Optimization]]