The key thing about writing parallel code is developing intuition.

## Cactus Stack

Cilk supports C's rules for pointers: a pointer to stack-allocated memory can be passed from parent to child, but not from child to parent (the child frame will die before the parent can use the pointer).

TODO: image of cactus stack

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

## DAGs, Work, and Span

TODO: image of trace dag

Assume for now each ==strand== (a sequence of instructions without a spawn, sync, or return from spawn) executes in unit time. 

> [!question]
> What is the parallelism of a computation? In other words, what is the maximum possible speedup for the parallel code compared to a serial execution?

> If 50% of your application is parallel and 50% is serial, you can't get more than a factor of 2 speedup, no matter how many processors it runs on.
> 
> — Amdahl's "Law"

> [!definition]
> Let $T_{p}$ be the execution time on $p$ processors. The ==work== of a program is defined as $T_{1}$, the time it would take for a single processor core to finish all tasks. The ==span== of a program is defined as $T_{\infty}$, the time to run the longest sequential path in the DAG.
> 
> Then, the ==parallelism== of the program is defined as $T_{1}/T_{\infty}$, i.e. the work over the span.

> [!claim] (Work and span laws)
> The work law states that $T_{p}\geq \frac{T_{1}}{p}$, while the span law states that $T_{p}\geq T_{\infty}$.

If two computations `A` and `B` are composed sequentially, then the work and span both add.

If instead `A` and `B` are composed in parallel, then the work adds while the spans are maxed together.

> [!definition]
> The ==speedup== of a program on $p$ processors is defined as $T_{1}/T_{p}$. If this is less than $p$, then it is ==sublinear==. If it is equal to $p$, then it is ==linear==. Note that ==superlinear== speedups are impossible in our model due to the work law.

Superlinear speedup could happen in real life if an increase in processor cores led to an increase in other resources (e.g. cache).

> [!claim] (Amdahl's Law)
> If $\alpha$ of $T_{1}$ must run serially, then the maximum speedup is
> $$
> \frac{T_{1}}{T_{p}}=\frac{T_{1}}{\alpha T_{1}+(1-\alpha)T_{1}/p}.
> $$
> This is because for the parallel runtime, $\alpha T_{1}$ work is spent in serial execution while $(1-\alpha)T_{1}$ work is spent in parallel execution, which is divided by $p$.

Note that the second term in the denominator is sometimes a poor approximation since it assumes that all the computation can actually be split perfectly across $p$ cores.

TODO: image of practical parallel algorithms

The goal is speedup. The tool is parallelism. And the problem is the races.

---

**Next:** [[Scheduling Theory and Parallel Loops]]