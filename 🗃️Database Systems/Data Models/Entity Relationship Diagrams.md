> An abstract representation of the information structure to be implemented in a database.

[[Tables, Tables, Tables#^ebd668|Entities]] are represented by rectangles. [[Tables, Tables, Tables#^778ef6|Attributes]] of these entities are represented by ovals. [[Tables, Tables, Tables#^03aa62|Primary keys]] are underlined attributes.

[[Tables, Tables, Tables#^ccb952|Relationships]] are represented by diamonds. The relationship can have a [[Tables, Tables, Tables#^ccb952|cardinalities]], e.g. `N:1` or `N:N`. One side of a relationship can also have a doubled line, meaning ==total participation== (every entity on that side must participate in the relationship). Relationships can even have their own attributes.

You can also have ==recursive relationships== between the same type of entity, e.g. management structure.

Here is an example of an ER diagram with all these bells and whistles:

![[er_diagram.png|center|384]]

> [!question]
> Translate the above ER diagram into a set of tables in a [[The Relational Data Model and Relational Algebra|relational database]].

> [!proof]- Solution
> Consider:
> * Keepers: (*kid*, age)
> * Cages: (*cid*)
> * Keeps: (*kid*, *cid*, keep_time)
> * Supervises: (*supervisor_kid*, supervisee_kid)

---

**Next:** [[Database Normalization]]
