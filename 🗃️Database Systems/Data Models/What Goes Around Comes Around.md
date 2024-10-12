> Notes from the paper [What Goes Around Comes Around](https://15721.courses.cs.cmu.edu/spring2020/papers/01-intro/whatgoesaround-stonebraker.pdf) and the associated [Red Book chapter](http://www.redbook.io/pdf/ch1-background.pdf).

This section is optional in terms of class material.
## Commentary

Learn from history, lest you suffer the same mistakes!

XML died because hierarchical data formats are bad, a lesson learned all too well by [[IMS|IMS]]. JSON may suffer the same fate, though it is more well-positioned as an alternate *data type* for representing *sparse data*, to be included in existing relational databases.

[[MapReduce]] has also failed as a data model since it fails to have any broad-scale applicability.

## Data Model Proposals

I only go through the first two data models, IMS and CODASYL. Feel free to read up on the rest of them.

1. [[IMS]]
2. [[CODASYL]]

## Recap of Lessons

1. Physical and logical data independence are highly desirable.
2. Tree structured data models are very restrictive.
3. It is a challenge to provide sophisticated logical reorganizations of tree structured data.
4. A record-at-a-time user interface forces the programmer to do manual query optimization, and this is often hard.
5. Networks are more flexible than hierarchies but more complex.
6. Loading and recovering networks is more complex than hierarchies.
7. Set-a-time languages are good, regardless of the data model, since they offer much improved physical data independence.
8. Logical data independence is easier with a simple data model than with a complex one.
9. Technical debates are usually settled by the elephants of the marketplace, and often for reasons that have little to do with the technology.
10. Query optimizers can beat all but the best record-at-a-time DBMS application programmers.
11. Functional dependencies are too difficult for mere mortals to understand. Another reason for KISS (Keep it simple stupid).
12. Unless there is a big performance or functionality advantage, new constructs will go nowhere.
13. Packages will not sell to users unless they are in "major pain".
14. Persistent languages will go nowhere without the support of the programming language community.
15. The major benefits of OR is two-fold: putting code in the data base (and thereby blurring the distinction between code and data) and user-defined access methods.
16. Widespread adoption of new technology requires either standards and/or an elephant pushing hard.

---

**Next:** [[The Relational Data Model and Relational Algebra]]