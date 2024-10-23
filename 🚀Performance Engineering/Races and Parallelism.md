The key thing about writing parallel code is developing intuition.

## Cactus Stack

Cilk supports C's rules for pointers: a pointer to stack-allocated memory can be passed from parent to child, but not from child to parent (the child frame will die before the parent can use the pointer).

![[cilk_cactus_stack.png|center|512]]

Cilk's cactus stack supports multiple views in parallel.

## Determinacy Races

> [!definition]
> A ==determinacy race== occurs when two logically parallel instructions access the same memory location and at least one of the instructions is a write.

> [!example] (Shared counter)
> This code exhibits a race:
> 
> ```c
> int x = 0;
> cilk_for (int i = 0; i < 2; ++i) {
> 	++x;
> }
> assert(x == 2);
> ```

> [!definition]
> The ==trace== of the execution of a program is a DAG of the computations.

^f37563

For the computation above, it looks like a diverging diamond: the root says `int x = 0;`, it branches into two paths that both execute `++x;`, and then they merge back to say `assert(x == 2);`.

Of course, each operation `++x;` is actually a sequence of atomic operations that first reads `x` into a register, increments the register, then writes it back into `x`. There are races between the writes and the other read/writes.

> [!example] (Types of races)
> Suppose that instructions `A` and `B` both access a memory location `x`, and suppose that `A` is parallel to `B`. These instructions form a ==read race== if one is a read and the other is a write, while they form a ==write race== if both are writes.

Meanwhile, two sections of code are ==independent== if they have no determinacy races between them.

## Avoiding Races

Iterations of a `cilk_for` should be kept independent. 

After calling a `cilk_spawn`, the child function should be independent of the further execution of the parent up to the next end of a `cilk_scope` (or a `cilk_sync`).

> [!warning]
> Machine word size matters, as races may occur in packed data structures. This is particularly nasty because it could depend on the compiler optimization level! For example, if
> 
> ```c
> struct {
> 	char a;
> 	char b;
> } x;
> ```
> 
> then updating `x.a` and `x.b` in parallel may cause a race!

> [!idea] (Cilksan race detector)
> One of the best ways to avoid races is to run the Cilksan race detector, i.e. compiling with `-fsanitize=cilk`. If the ostensibly deterministic Cilk program could possibly behave differently on a given input than its serial projection, Cilksan *guarantees* to report and localize the given race.

---

**Next:** [[DAGs, Work, and Span]]
