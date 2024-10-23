Here is a picture of a trace DAG:

![[trace_dag.png|center|512]]

Assume for now each ==strand== (a sequence of instructions without a spawn, sync, or return from spawn) executes in unit time.  ^b3b644

> [!question]
> What is the parallelism of a computation? In other words, what is the maximum possible speedup for the parallel code compared to a serial execution?

> If 50% of your application is parallel and 50% is serial, you can't get more than a factor of 2 speedup, no matter how many processors it runs on.
> 
> — Amdahl's "Law"

> [!definition] (Work, span, and parallelism)
> Let $T_{p}$ be the execution time on $p$ processors. The ==work== of a program is defined as $T_{1}$, the time it would take for a single processor core to finish all tasks. The ==span== of a program is defined as $T_{\infty}$, the time to run the longest sequential path in the DAG.
> 
> Then, the ==parallelism== of the program is defined as $T_{1}/T_{\infty}$, i.e. the work over the span.

^591a3b

> [!claim] (Work and span laws)
> The ==work law== states that $T_{p}\geq \frac{T_{1}}{p}$, while the ==span law== states that $T_{p}\geq T_{\infty}$.

^935a5c

If two computations `A` and `B` are composed sequentially, then the work and span both add.

If instead `A` and `B` are composed in parallel, then the work adds while the spans are maxed together.

> [!definition] (Speedup)
> The ==speedup== of a program on $p$ processors is defined as $T_{1}/T_{p}$. If this is less than $p$, then it is ==sublinear==. If it is equal to $p$, then it is ==linear==. Note that ==superlinear== speedups are impossible in our model due to the work law.

Superlinear speedup could happen in real life if an increase in processor cores led to an increase in other resources (e.g. cache).

> [!claim] (Amdahl's Law)
> If $\alpha$ of $T_{1}$ must run serially, then the maximum speedup is
> $$
> \frac{T_{1}}{T_{p}}=\frac{T_{1}}{\alpha T_{1}+(1-\alpha)T_{1}/p}.
> $$
> This is because for the parallel runtime, $\alpha T_{1}$ work is spent in serial execution while $(1-\alpha)T_{1}$ work is spent in parallel execution, which is divided by $p$.

Note that the second term in the denominator is sometimes a poor approximation since it assumes that all the computation can actually be split perfectly across $p$ cores.

![[practical_parallel_algos.png|center|512]]

The goal is speedup. The tool is parallelism. And the problem is the races.

---

**Next:** [[Scheduling Theory and Parallel Loops]]