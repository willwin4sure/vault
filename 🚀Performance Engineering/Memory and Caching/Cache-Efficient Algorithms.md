> Caching is intellectually more challenging than parallelism. It's time to start with the harder stuff.

Here's a reminder of the multicore cache hierarchy:

![[multicore_cache_hierarchy.png|center|512]]

Each large box is a processor chip, which contains multiple cores with individual L1 and L2 caches, and a shared L3 cache. When you miss the last level cache, you go to the memory controller to fetch from DRAM, which is rather slow. When you have multiple processor chips on the same memory network, another faster option is to request the data from the cache of another chip through the network.

> [!idea]
> Missing cache and going to DRAM costs ~3000 floating point operations, so it is vital to write code that uses cache well.

Here are some cache specs for the Intel Xeon Platinum 8280L (Cascade Lake), a high-end multicore. ^69f253

![[cascade_lake_cache_specs.png|center|512]]

> [!warning]
> One funny reason why manufacturers don't like releasing exact specs is because they don't want software to become too well-tuned to the last generation of hardware, since when they release a new version it might cause code to slow down.

First, let's go review cache architectures. ^853326

1. [[Cache Associativity]]

## Conflict Misses for Submatrices

Conflict misses can be problematic for caches with limited associativity. One example is when you are accessing the elements of the submatrices of a matrix, which will have a stride equal to the width of the full matrix.

If the width of the matrix is a large power of 2, then all columns of the submatrix will be mapped to the same set, causing conflict misses and underutilizing the cache!

> [!idea]
> This is why the L3 cache above is 11-way set associative, which will prevent this issue. It can also be solved by padding your large matrix (e.g. into 1025 per row so the data is stored differently), or by copying your submatrix out, assuming the work you'll be doing is large enough.

## Ideal-Cache Model

^50918e

We first deal with idealized caches. It has $\mathcal{M}$ bytes total and $\mathcal{B}$ bytes per block, is fully associative, and is optimal with omniscient replacement (e.g. always kicks out the cache block whose next access is as far in the future as possible).

![[ideal_cache.png|center|384]]

We will measure performance in terms of the work $T$ and the number of cache misses $Q$.

> [!claim] ("LRU" lemma)
> Suppose that an algorithm incurs $Q$ cache misses on an ideal cache of size $\mathcal{M}$. Then on a fully associative cache of size $2\mathcal{M}$ that uses the LRU replacement policy, it incurs at most $2Q$ cache misses.

This means that when analyzing the asymptotics of caches, we can choose either the omniscient policy or the LRU policy and they agree up to a constant factor.

> [!warning] (Real world details)
> After designing a theoretically good algorithm, you will need to engineer some details:
> * Real caches are [[Cache Associativity#Set-Associative Caches|set associative]].
> * Loads and stores have different costs with respect to bandwidth and latency.

> [!claim] (Segment caching lemma)
> Suppose that a program reads a set of $r$ data segments, where the $i$-th segment consists of $s_{i}$ contiguous bytes in memory, and suppose that
> $$
> \sum_{i=1}^{r}s_{i}=N<\mathcal{M}/3\quad \text{and}\quad N/r\geq \mathcal{B}.
> $$
> Then all of the segments fit into cache, and the number of misses to read them all is at most $3N/\mathcal{B}$.

^e950d6

> [!proof]-
> A single segment $s_{i}$ incurs at most $\frac{s_{i}}{\mathcal{B}}+2$ misses since there are two ends. So the total number of misses is at most
> $$
> \sum_{i=1}^{r}\left[ \frac{s_{i}}{\mathcal{B}}+2 \right]=\frac{n}{\mathcal{B}}+2r\leq \frac{3n}{\mathcal{B}}.
> $$

Here is a useful assumption for some analyses:

> [!definition] (Tall-cache assumption)
> This states that $\mathcal{B}^{2}\leq c\mathcal{M}$ for some sufficiently small constant $c\leq 1$.

Basically, you have more lines than bytes in a line; it is tall. Sometimes you want smaller $c$ as well. In practice, this assumption is usually satisfied, e.g. in Xeon Platinum cache lines are $2^{6}$ bytes while the L1 cache size is $2^{15}$ bytes.

A notable exception is that [[translation lookaside buffers]], a kind of cache, are actually short.

> [!example] (What's wrong with short caches?)
> One example is with submatrix analysis: an $n\times n$ submatrix might not fit in a short cache even if $n^{2}\leq c\mathcal{M}$!

> [!claim] (Submatrix caching lemma)
> Suppose that an $n\times n$ submatrix $A$ is read into a tall cache satisfying $\mathcal{B}^{2}<c\mathcal{M}$, where $c<1/3$, and suppose $c\mathcal{M}\leq n^{2}<\mathcal{M}/3$. Then $A$ fits into the cache, and the number of misses to read all of $A$'s elements is at most $3n^{2}/\mathcal{B}$.

This follows directly from the [[Cache-Efficient Algorithms#^e950d6|segment caching lemma]] above. 

## Cache Analysis of Matrix Multiplication

### Naive Method

Here is the naive implementation:

```c
void mult(double* C, double* A, double* B, int64_t n) {
	for (int64_t i = 0; i < n; ++i) {
		for (int64_t j = 0; j < n; ++j) {
			for (int64_t k = 0; k < n; ++k) {
				C[i*n+j] = A[i*n+k] * B[k*n+j];
			}
		}
	} 
}
```

Assume row-major storage and a tall cache that uses LRU. We will analyze the matrix `B`, since that is where the majority of the cache misses occur, since we are striding in the wrong direction.

1. **Case 1:** $n\geq \mathcal{M}/\mathcal{B}$. In this case, every stride incurs a cache miss on every element. So there are $\mathcal{O}(n^{3})$ cache misses in this entire program.
2. **Case 2:** $\mathcal{M}^{1/2}\leq n < \mathcal{M}/\mathcal{B}$. In this case, the first stride of each cache block width causes a cold miss on every row, but each subsequent stride in this block incurs no misses. So there are $\mathcal{O}(n^{3}/\mathcal{B})$ cache misses in this entire program.
3. **Case 3:** $n<c\mathcal{M}^{1/2}$ where $c\leq \frac{1}{3}$. Now the submatrix caching lemma says all of `B` fits so we just incur $\mathcal{O}(n^{2}/\mathcal{B})$ cache misses.

We are ignoring the additional cases in between since they don't give an extra intuition.

> [!idea]
> The main phase transition when actually engineering performance is between "everything fits" and "everything doesn't fit".

> [!question]
> What happens to the cache misses when you swap the `j` and `k` loops?

### Tiled Method

Now, let's try a tiled implementation (you probably used tiling on Project 1 when rotating a bit matrix). 

```c
void tiled_mult(double* C, double* A, double* B, int64_t n) {
	for (int64_t i1 = 0; i1 < n/s; i1 += s) {
		for (int64_t j1 = 0; j1 < n/s; j1 += s) {
			for (int64_t k1 = 0; k1 < n/s; k1 += s) {
				for (int64_t i = i1; i < i1 + s; ++i) {
					for (int64_t j = j1; j < j1 + s; ++j) {
						for (int64_t k = k1; k < k1 + s; ++k) {
							C[i*n+j] += A[i*n+k] * B[k*n+j];
						}
					}
				}
			}
		}
	}
}
```

We want to tune the cache size `s` so that it just fits in cache, resulting in $\mathcal{O}\left( \frac{n^{3}}{\mathcal{B}\mathcal{M}^{1/2}} \right)$ cache misses. TODO: explain why. This is asymptotically optimal. 

> [!idea]
> When you get a $\mathcal{B}$ in the denominator of your number of cache misses $Q$, that indicates effective use of spatial locality. If you get a $\mathcal{M}$ in the denominator, you are using temporal locality.

This is sometimes called a ==voodoo== parameter, since it is highly dependent on the particular machine that you run on. It's also not quite intuitively optimal since you probably want more tiling for your different levels of caches.

### Divide and Conquer Method

We've [[Task-Parallel Algorithms#Matrix Multiplication|seen this before]]. Now the number of cache misses is given by the recurrence
$$
Q(n)=\begin{cases}
\Theta(n^{2}/\mathcal{B}) & \text{if }n^{2}<c\mathcal{M} \text{ for suff. small }c\leq 1, \\
8Q(n/2)+ \Theta(1) & \text{otherwise}.
\end{cases}
$$
Solving this via the recursion tree gives $Q(n)=\Theta(\frac{n^{3}}{\mathcal{B}\mathcal{M}^{1/2}})$. This is still asymptotically optimal while being oblivious to the cache size! We'll talk more about such algorithms in the next lecture.

Here are some benefits:

* No voodoo parameters!
* No explicit knowledge of caches.
* Passively autotuned.
* Handles multi-level caches automatically.
* Good in multitenancy environments.

> [!theorem] (Cilk and caching)
> Let $Q_{p}$ be the number of cache misses in a deterministic Cilk computation when run on $p$ processors, each with private cache size $\mathcal{M}$. In the ideal-cache model, we have
> $$
> Q_{p}=Q_{1}+\mathcal{O}(S_{p}\mathcal{M}/\mathcal{B}),
> $$
> where $\mathcal{M}$ is the cache size, $\mathcal{B}$ is the cache-block size, and $S_{p}$ is the number of processor-processor migrations (steals) during the computation.

> [!proof]-
> After a worker steals a continuation, its cache is completely cold in the worst case. But after $\mathcal{M}/\mathcal{B}$ cold cache misses, its cache is identical to that in the serial execution. The same is true when a worker resumes a stolen subcomputation after exiting a `cilk_scope`. The number of times these two situations can occur is at most $2S_{p}$.

In practice, minimizing cache misses in serial projections tends to minimize them too in parallel execution.

---

**Next:** [[Cache-Oblivious Algorithms]]