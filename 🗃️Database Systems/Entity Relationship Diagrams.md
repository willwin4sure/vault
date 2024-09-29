

==Entities== are represented by rectangles.

==Attributes== of these entities are represented by ovals.

Primary keys are underlined attributes.

==Relationships== are represented by diamonds. The relationship can have a cardinality, e.g. `N:1` or `N:N`. One side of a relationship can also have a double line, meaning total participation (every entity on that side must participate in the relatinoship). Relationships can even have their own attributes.

You can also have recursive relationships between the same type of entity.

---

Redundancy is not necessarily bad because of duplicated storage (nowadays, storage is cheap)! Rather, anomalies may appear from bad updates (what if you don't edit everything and the state of the database is invalid?). 

==Normalizing== a database makes it redundancy free, i.e. remove as many potential anomalies as possible. 

---

A ==functional dependence== $X \to Y$ means that attributes $X$ uniquely determine $Y$. These are properties of the application, not the data.

---

A set of relations and functional dependencies is in ==Boyce-Codd Normal Form== (BCNF) if and only if: every FD is either:

1. Trivial (e.g. $Y$ contains $X$),
2. $X$ is a key of the table.

After all, if an FD violates 2, multiple rows with same $X$ value may occur.


If you give me a schema $R$ and a set $F$ of functional dependencies, we can BCNF-ify it with the following pseudocode:

```
D = {(R, F)}
while exists (schema, FD set) pair (S, F') in D not in BCNF:
	given X -> Y as a BCNF-violating dependency in F'
	replace (S, F') in D with
		S1 = (XY, F1) and
		S2 = ((S-Y) union X, F2)
	where F1 and F2 are the FDs in F' over XY or (S-Y) union X
```

You may lose dependencies (?!).

==Third Normal Form== (3NF). 3NF will eliminate as much redundancy as possible while preserving all dependencies!


