> Deciding how to store and access your data.

Choosing a data model is a fundamental first step to building a database management system. Let's start with a basic example of building a database for a Zoo. In this model, we will use tables to express all kinds of different relationships.

1. [[Tables, Tables, Tables]]

People have tried many different ways of expressing data, of course. 

> [!idea]
> The moral of the story is that the relational model is a strong battle-worn solution.

But you can dig into the details and ensure you don't repeat history's mistakes. Another useful more modern paper is  [What Goes Around Comes Around... and Around...](https://db.cs.cmu.edu/papers/2024/whatgoesaround-sigmodrec2024.pdf).

2. [[What Goes Around Comes Around]]

Every data model requires an associated query language to interact with it. The default for relational models is called SQL.

> [!idea]
> SQL queries declare at a logical level *what* data is desired, without any ties to *how* the database management system actually gets it.

3. [[The Relational Data Model and Relational Algebra]]
4. [[SQL]]
5. [[Fancy SQL]]

Now let's dive a bit more into database design. We'll also discuss normalization techniques to keep a database redundancy free; this is vital for transactional databases but less so for analytical ones.

5. [[Entity Relationship Diagrams]]
6. [[Database Normalization]]

---

**Next:** [[⛺Query Processing Homepage]]