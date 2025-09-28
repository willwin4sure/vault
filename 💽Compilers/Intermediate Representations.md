IR refers to a lot of things:

* Parse trees
* Abstract syntax trees
* Basic blocks
* Control flow graphs
* Static single-assignment form
* Directed acyclic graphs

The goal of high-level IR is for **semantic checking** and **program analysis**.

## Symbol Tables and Scopes

You augment your abstract syntax tree with ==symbol tables==, which map from identifiers (e.g. `x` or `f`) to descriptors (e.g. are the local variables or methods, what are their types).

These are held in ==scopes==. There is a global scope, as well as various method and block scopes. Scopes know about their parents, and when you lookup some identifier you walk up the parents until you find it (or report a semantic error). This does mean in nested scopes you can shadow identifiers in the parent.

Descriptors in symbol tables will contain information about children in the tree, e.g. methods or classes. Scopes know about parents so you can go up the tree.

## Control Flow Graphs

You've seen these in 6.191.

These consist of basic blocks, which have no control flow within them (you must enter basic blocks from the top; once you do, you must execute the instructions in order and exit out the bottom). These should be maximal. Edges between these basic blocks represent control flow as (conditional) jumps.