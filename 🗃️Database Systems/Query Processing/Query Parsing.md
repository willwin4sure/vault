The first step of query processing: this simply converts SQL code into a parse tree of operations; standard 6.1010 fare.

```mermaid
graph TD;
id0 ~~~ id1;
id1 --> id0;
id1 ~~~ id0;

id1 ~~~ id2;
id2 --> id1;
id2 ~~~ id1;

id2 ~~~ id3;
id3 --> id2;
id3 ~~~ id2;

id2 ~~~ id4;
id4 --> id2;
id4 ~~~ id2;

id0(project)
id1(select)
id2(join)
id3(table1)
id4(table2)
```

Here's a basic parse tree for a potential query. We read the tables at the bottom, join them, select the rows we want, then project to the columns we want.

Note that multiple query trees can correspond to the same query. For example, you'll probably want to [[The Relational Data Model and Relational Algebra#^d64f56|predicate push-down]] the select to under the join, assuming that it is a function of the columns of one of the tables:

```mermaid
graph TD;
id0 ~~~ id1;
id1 --> id0;
id1 ~~~ id0;

id1 ~~~ id2;
id2 --> id1;
id2 ~~~ id1;

id2 ~~~ id3;
id3 --> id2;
id3 ~~~ id2;

id1 ~~~ id4;
id4 --> id1;
id1 ~~~ id1;

id0(project)
id1(join)
id2(select)
id3(table1)
id4(table2)
```

This is a responsibility of the next few sections.

---

**Next:** [[Query Rewriting]]
