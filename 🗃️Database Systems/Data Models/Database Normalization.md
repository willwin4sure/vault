> Structuring a database to reduce data redundancy and improve data integrity.

Nowadays, redundancy is not necessarily a bad thing because of duplicated storage (storage is insanely cheap!). Rather, ==anomalies== may appear from bad updates (what if the database is accidentally left in an invalid state?).

> [!definition] (Database normalization)
> ==Normalizing== a database makes it redundancy free, i.e. by removing as many potential anomalies as possible. 

## Boyce-Codd Normal Form

^00daa2

This removes all redundancy based on functional dependencies.

> [!definition] (Functional dependence)
> A ==functional dependence== $X \to Y$ means that attributes $X$ uniquely determine $Y$. These are properties of the application, not the data.

> [!definition] (Boyce-Codd normal form)
> A set of relations and functional dependencies is in ==Boyce-Codd Normal Form== (BCNF) if and only if the following condition holds: every functional dependence $X\to Y$ is either
> 
> 1. Trivial (e.g. $Y$ contains $X$),
> 2. $X$ is a key of the table.
> 
> After all, if a nontrivial FD violates 2, multiple rows with same $X$ value may occur.

If you give me a schema $R$ and a set $F$ of functional dependencies, we can convert it into BCNF form with the following pseudocode:

```
D = {(R, F)}
while exists (schema, FD set) pair (S, F') in D not in BCNF:
	given X -> Y as a BCNF-violating dependency in F'
	replace (S, F') in D with
		S1 = (XY, F1) and
		S2 = ((S-Y) union X, F2)
	where F1 and F2 are the FDs in F' over XY and (S-Y) union X, resp.
```

Basically, you yank out the dependency $X\to Y$ into its own table, and remove anything in $Y$ from the existing table, and add in all of $X$.

> [!warning]
> This process may result in loss of functional dependences!

## Third Normal Form

This is a weaker normalization form than [[Database Normalization#^00daa2|BCNF]]. ==Third normal form (3NF)== will eliminate as much redundancy as possible while preserving all dependences!

---

**Return:** [[⛺Data Models Homepage]]

