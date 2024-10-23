> Tables are a great way to store data.

## The Zoo Data Model

You are a database engineer at a zoo. The internal website needs to display and update various data, described in the following abstract model called an [[Entity Relationship Diagrams|entity relationship diagram]]. 

![[er_zoo.png|center|512]]

The three core ==entities== are animals, cages, and keepers. Entities may be assigned properties:

* Animals have names, ages, and species.
* Cages have feeding times and buildings.
* Keepers have names.

> [!warning]
> One design decision expressed by the diagram is that feeding times are associated with cages rather than animals. This could depend on the requirements of the zoo.

These entities will also have ==relationships== with each other.

* Cages contain animals. Animals are in exactly one cage, while cages may have multiple animals. This is called a ==N-to-1 relationship== with ==total participation== from animals.
* Keepers manage cages. Keepers may manage multiple cages, and cages may be managed by multiple keepers. This is called an ==N-to-N relationship==. ^3237f0

We will go over these definitions in more detail when we discuss [[Entity Relationship Diagrams|entity relationship diagrams]] in more detail.

## Entity Storage

Entities can be easily stored inside tables, along with their respective properties. For example, here is my animals table, which is indexed by the ==primary key== *`animal_id`*.

| *`animal_id`* | `name` | `age` | `species` | `cage_id` |
| ------------- | ------ | ----- | --------- | --------- |
| 1             | Tim    | 13    | Giraffe   | 1         |
| 2             | Mike   | 3     | Moose     | 2         |
| 3             | Sally  | 1     | Student   | 1         |

Since *`animal_id`* is a primary key, it uniquely defines each row of the table. We never see two animals with the same ID. ^03aa62

> [!idea]
> Entities can be stored in a table with columns as their attributes. It is often useful to include an ID column.

We have captured the N-to-1 relationship with cages by including a ==foreign key== `cage_id`, which points to the corresponding field in the cages table, shown below.

| *`cage_id`* | `feed_time` | `building` |
| ----------- | ----------- | ---------- |
| 1           | 1:30        | 1          |
| 2           | 2:30        | 2          |

If marked as a foreign key, the relational database will maintain the guarantee that the cage ID assigned to an animal is indeed a valid cage ID. It will also give warnings if you, say, try to delete a cage that animals are in.

> [!idea]
> N-to-1 relationships can be modeled by including the primary key of the 1 side of the relationship (the cage) as a foreign key in the N side of the relationship (the animals).

## Relationship Storage

We've already seen how to capture N-to-1 relationships above using a primary and foreign key. How would we encode N-to-N relationships?

One idea could be to store lists in our columns, but this is a poor solution as we'd prefer our tuples to have fixed size. Here is a better solution using a table of pairs of foreign keys:

| *`keeper_id`* | *`cage_id`* |
| ------------- | ----------- |
| 1             | 1           |
| 1             | 2           |
| 2             | 1           |

> [!idea]
> N-to-N relationships can be modeled by just storing all the edges in a table.

Note here that *`keeper_id`* and *`cage_id`* form the primary key pair, meaning that together they uniquely define each row in the table. They are also both foreign keys to the keeper and cage tables, respectively (the keeper table is not shown).

---

**Next:** [[What Goes Around Comes Around]]