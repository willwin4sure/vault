We've seen that [[Cilk]] allows the programmer to express the *potential parallelism* in an application. The Cilk ==scheduler== maps strands onto processors dynamically at runtime.

Cilk uses a distributed work-stealing algorithm. This is complicated, so for this class we'll explore the ideas in a simple centralized scheduler.

## Greedy Scheduling

> Scheduling greedily is 2-competitive.

> [!idea] (Greedy scheduling)
> Schedule as much as possible on every step!

For simplicity, we will consider [[Races and Parallelism#^f37563|trace]] DAGs where every [[DAGs, Work, and Span#^b3b644|strand]] requires unit time (you can simply split your nodes if necessary). We will also ignore any scheduling overhead.

> [!definition] (Ready strands)
> Call a strand in a trace ==ready== if all its predecessors have executed.

> [!definition] (Greedy scheduler steps)
> Define a ==complete step== as any step where at least $P$ strands are ready, and we run *any* $P$ of them. An ==incomplete step== is any step where less than $P$ strands are ready, and we run all of them.

> [!theorem] (Greedy scheduling theorem)
> Any greedy scheduler achieves
> $$
> T_{p}\leq \frac{T_{1}}{p}+T_{\infty}.
> $$
> [[DAGs, Work, and Span#^591a3b|Recall]] that $T_{p}$ represents the execution time on $p$ processors.

^f22f4b

> [!proof]-
> Every complete step decreases the work of the unexecuted DAG by $p$ , which means that there are at most $\frac{T_{1}}{p}$ complete steps.
> 
> Every incomplete step decreases the span of the unexecuted DAG by $1$, which means that there are at most $T_{\infty}$ incomplete steps.

> [!claim] (Greedy scheduling is 2-competitive)
> Recall from the [[DAGs, Work, and Span#^935a5c|work and span laws]] that at optimum, $T_{p}^{*}\geq \max\left( \frac{T_{1}}{p},T_{\infty} \right)$. We've shown that greedy takes at most $\frac{T_{1}}{p}+T_{\infty}\leq 2\cdot \max\left( \frac{T_{1}}{p}, T_{\infty} \right)$.

> [!question]
> Modify the argument above to prove the stronger bound
> $$
> T_{p}\leq \frac{T_{1}-T_{\infty}}{p}+T_{\infty}.
> $$

> [!proof]-
> The number of steps (complete or incomplete) that reduce the span of the unexecuted DAG is at most $T_{\infty}$​. The only remaining steps are complete (since all incomplete steps reduce the span of the unexecuted DAG), and they have $T_{1}-T_{\infty}$ work left to do, which takes $\frac{T_{1}-T_{\infty}}{p}$ time.

> [!claim] (High parallelism gives near-linear speedup)
> Any greedy scheduler achieves near-perfect linear speedup whenever $T_{1}/T_{\infty}\gg p$.

> [!proof]-
> Well, $T_{\infty}\ll \frac{T_{1}}{p}$, so the [[Scheduling Theory and Parallel Loops#^f22f4b|greedy scheduling theorem]] tells us that $T_{p}\leq \frac{T_{1}}{p}+T_{\infty}\approx \frac{T_{1}}{p}$ so the speedup is $\frac{T_{1}}{T_{p}}\approx p$.

The quantity $\frac{T_{1}/T_{\infty}}{p}$ is called the ==parallel slackness==. A good rule-of-thumb is a parallel slackness of a factor of $10$. 

> [!idea]
> You want more logical parallelism in your code (in terms of chunks, say) than the number of physical processors that you actually have! This allows effective load-balancing.

## Cilk's Scheduler

Provably, Cilk's randomized work-stealing scheduler achieves $T_{p}=\frac{T_{1}}{p}+\mathcal{O}(T_{\infty})$ expected time (the overhead occurs on the incomplete steps). Empirically, it achieves around $T_{p}\approx \frac{T_{1}}{p}+T_{\infty}$ time.

So you also get near-perfect linear speedup as long as $T_{1}/T_{\infty}\gg p$.

## Parallel Loops

> [!idea]
> Cilk actually only supports binary spawning, so parallel loops are formed using a binary tree. This adds $O(n)$ work and $O(\log n)$ span for the internal nodes of the tree.

Here's an example of a parallel matrix transpose:

```c
cilk_for (int i = 1; i < n; ++i) {
	for (int j = 0; j < i; ++j) {
		double temp = A[i][j];
		A[i][j] = A[j][i];
		A[j][i] = temp;
	}
}
```

In this case, the loop overhead doesn't change the asymptotic work and spans; they remain at $\Theta(n^{2})$ and $\Theta(n)$, respectively.

However, if we parallelize both loops, we get

```c
cilk_for (int i = 1; i < n; ++i) {
	cilk_for (int j = 0; j < i; ++j) {
		double temp = A[i][j];
		A[i][j] = A[j][i];
		A[j][i] = temp;
	}
}
```

Now, the work is still $\Theta(n^{2})$, but the span is now $\Theta(\log n)$ (only exists from loop overhead). The excess parallelism doesn't really help if our processors are already saturated anyway: we want the bottom to be more coarsened.

Cilk actually allows a way of coarsening parallel loops; we can write the following pragma right before the `cilk_for` line. If omitted, the compiler will coarsen it automatically and usually make a decent choice.

```c
#pragma cilk grainsize G
cilk_for (int i = 0; i < n; ++i) {
	A[i] += B[i];
}
```

Suppose $I$ is the cost of each loop body iteration and $S$ is the cost of the spawning in each internal node. Then, the total work and span are
$$
T_{1}=n\cdot I+(n/G - 1) \cdot S,\quad T_{\infty}=G\cdot I+\log(n/G)\cdot S.
$$
As $G$ increases, the parallel loop over head decreases but the span potentially increases.

We want $G \gg S/I$ to minimize the work overhead, but we also want to keep $G$ small for shorter span.

## Rules of Thumb

1. Minimize the span to maximize parallelism. Try to generate 10 times more parallelism than processors.
2. If you have plenty of parallelism, try to trade some of it off for less work overhead.
3. Use divide-and-conquer recursion or parallel loops for large-scale spawning.
4. Ensure that work/#spawns is sufficiently large.
5. Parallelize outer loops as opposed to inner loops.
6. Watch out for scheduling overheads.

---

**Next:** [[Task-Parallel Algorithms]]