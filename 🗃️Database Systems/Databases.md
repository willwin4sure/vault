A ==database== is a structured data collection holding records and their relationships. This class is about database management systems.

> [!example]
> AWS Aurora is a cloud-managed OLTP DBMS. 

## Case Study: Zoo Data Model

Suppose you are a database engineer for a zoo. 

![[er_zoo.png|center|512]]

Three core ==entities== will be animals, cages, and keepers. These have properties:

* Animals have names, ages, and species.
* Cages have feeding times and buildings.
* Keepers have names.

Note a design choice is that feeding times are associated with cages rather than animals. This could depend on requirements of the zoo.

Entities will also have ==relationships== with each other.

* Cages contain animals. Animals are in exactly one cage, while cages may have multiple animals.
* Keepers manage cages. Keepers may manage multiple cages, and cages may be managed by multiple keepers.

Entities can be stored inside tables. Here are my animals:

| ID  | Name  | Age | Species | Cage ID |
| --- | ----- | --- | ------- | ------- |
| 1   | Tim   | 13  | Giraffe | 1       |
| 2   | Mike  | 3   | Moose   | 2       |
| 3   | Sally | 1   | Student | 1       |

The database must provide the relational guarantee that the cage ID assigned to an animal must be the ID of an actual cage.

| Cage ID | Feed Time | Building |
| ------- | --------- | -------- |
| 1       | 1:30      | 1        |
| 2       | 2:30      | 2        |

Handling the relationship between cages and keepers is more challenging, since there is a many-to-many mapping. We could simply use list values, but those are not generally permitted.

One solution is to just store the edges in a table.

| Keeper ID | Cage ID |
| --------- | ------- |
| 1         | 1       |
| 1         | 2       |
| 2         | 1       |

Of course, the keeper IDs must be assigned as well in the keeper table.

## Structured Query Language

Some examples:

```sql
SELECT field1, ..., fieldN
FROM table1, ...
WHERE condition1, ...
```

```sql
INSERT INTO table VALUES (field1, ...)
```

```sql
UPDATE table SET field1 = X, ...
WHERE condition1, ...
```

Rather than **imperatively** telling the database what to do, we **declaratively** ask for some specification that the implementation returns us.

When we have a query that involves multiple tables, the most naive implementation would iterate through the entire Cartesian product. However, ==join== clauses in SQL will allow more efficient implementations.

You may also want to ==self-join== a table to itself if answering a question about pairs of elements of the table. SQL also supported ==nested queries==. 

> [!example] (Find keepers who keep both students and salamanders)
> 
> ```sql
> SELECT keeper.name
> FROM keeper, cages as c1, cages as c2,
> 	keeps as k1, keeps as k2, animals as a1, animals as a2
> WHERE c1.cageid = k1.cageid AND keeper.keeperid = k1.keeperid
> AND c2.cageid = k2.cageid AND keeper.keeperid = k2.keeperid
> AND 
> ```

Optimizations allowed by declarative queries:
- Sorting by type (insertions are slower).
- Store things in a hash table or tree.

SQL -> procedural plan -> optimized plan -> compiled program.

==Predicate push-down==: move filtering to lower (earlier) parts in the procedure tree.