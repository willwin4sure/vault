> Hierarchical models are restrictive, and IMS + DL/1 forced the programmer both to know too much about the underlying data representation and to manually optimize the query.

Information Management System (IMS) works with the notion of a ==record type==, which is a collection of named fields with associated data types. Any ==instance== of a record type is a tuple of elements obeying this data description.

Some subset of the named fields must uniquely specify a record instance, i.e. they are required to be a ==key== (we call this a [[Tables, Tables, Tables#^03aa62|primary key]]).

## Hierarchical Model

> [!idea]
> The main difference of IMS is that record types are arranged in a *tree structure*, such that each record type has a unique parent record type.

> [!warning]
> Hierarchical models present key challenges for modeling [[Tables, Tables, Tables#^3237f0|N-to-N relationships]]:
> 
> 1. **Information is repeated.** One of the two entities is marked as the parent, and then children will be duplicated across different parent record types.
> 2. **Existence depends on parents.** The inner entity cannot exist without some outer entity to attach itself to.

## DL/1 Query Language

The reason IMS uses a hierarchical design is because it supports a simple data manipulation language called ==DL/1==. 

Every record in an IMS database can be identified by a ==hierarchical sequence key==, which is just a concatenation of all the keys of ancestor records down from the root. This defines a natural DFS order that DL/1 interacts with.

In particular, the language DL/1 is *stateful*: there is a current pointer within the tree that the commands interact with. For example "get next" grabs the next record in HSK order, while "get next within parent" explores the subtree under the current record. 

```DL/1
Get unique Supplier (sno=16)
Until failure do
	Get next within parent (color=red)
	Enddo
```

Above is DL/1 pseudocode for fetching all red parts supplied by supplier number 16. The programmer has to juggle between multiple possible implementations and make optimization trade-offs based on the data and structure of the database.

```DL/1
Until failure do
	Get next Part (color=red)
	Enddo
```

For example, the code above appears to do a different thing than the previous query, but it would be a faster implementation if there is only one supplier.

## Physical Storage Formats

In IMS, records in the root node can be stored in multiple ways:

* Sequentially.
* Indexed in a B-tree using the key of the record.
* Hashed using the key of the record.

Dependent records are found from the root using either:

* Sequentially adjacent.
* Various forms of pointers.

The physical representation of the data limits what kinds of operations the programmer can perform. For example, a pure sequential organization will not support record inserts, and hashed root records will not support "get next".

> [!warning]
> IMS and DL/1 fail to have ==physical data independence==, the property that the physical structure of the database shouldn't affect the logical structure. This is because changing the underlying data representation may invalidate certain queries.

^ff98ad

## Logical Requirements

What if the logical requirements of the database change over time? IMS does actually support a level of ==logical data independence==, the ability to change the logical structure of the database without affecting external user views. ^8b1c2b

This is because DL/1 is actually defined on a logical database sitting above the physical database that is actually stored. If we change the physical database representation, we can continue to support older queries by simply defining the logical database to be a subtree of the full physical database.

## N-to-N Relationships

Earlier, we were complaining that N-to-N relationships can't be easily expressed with tree-based models without incurring redundancy and existence dependency.

> [!idea]
> The separation of logical and physical databases does allow IMS to support N-to-N relationships by presenting the user with a view that is different than the underlying representation.

Physically, the database can store one table (`Supplier`) and the relationship table (`Supplies`) as its child, as well as the other table (`Part`) separately.

DL/1 operates on trees so cannot interact with this data. However, it will be presented with the fused hierarchical view where the relevant parts are fused onto the supply relationship records.

---

**Next:** [[CODASYL]]