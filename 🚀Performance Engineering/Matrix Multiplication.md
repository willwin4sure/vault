> Case study on matrix multiplication.

Suppose we are interested in multiply two $n$-by-$n$ matrices $A$ and $B$ to get a result $C$. This amounts to
$$
c_{ij}=\sum_{k=1}^{n}a_{ik}b_{kj},
$$
which naively takes $\mathcal{O}(n^3)$ operations. This is the foundation of all machine learning models, so optimizing the performance is vital.

## Hardware Constraints

![[specs.png|center|512]]

The peak performance can be calculated as
$$
(2.9 \cdot 10^9)\cdot 2 \cdot 9 \cdot 16 = 836\text{ GFLOPS}.
$$
If $n=2^{12}$, then we need $2n^{3}=2^{37}$ floating point operations. 

## Basic Implementations

We are comparing naive implementations that just use a triple for loop:

```c
for (int i = 0; i < n; ++i) {
	for (int j = 0; j < n; ++j) {
		for (int k = 0; k < n; ++k) {
			C[i][j] += A[i][k] * B[k][j];
		}
	} 
}
```

Python takes 6 hours, which is around 6.25 MFLOPS. We are getting 0.00075% of the peak performance. This is because it is ==interpreted==.

Java takes 46 minutes, which is around 8.8x faster than Python. This uses JIT, which is initially interpreted but compiles pieces of code that are executed sufficiently frequently.

C takes 19 minutes, which is 18x faster than Python. But we're still only getting 0.014% of the peak performance. This is so much faster because it is ==compiled==.

## Optimizations

> [!example] (Loop order)
> We can permute the order of the loops in the naive implementation. The fastest loop orders are `i, k, j` or `k, i, j`, and confer an 18x speedup! This is due to caching. We want to optimize for locality where matrices are stored in row-major order.

> [!example] (Compiler flags)
> Try `-O2` or `-O3` with the compiler. The reason `-O0` exists is because the compiled code will be most similar to your original code and therefore easy to debug. There are other flags like `-Os` to limit code size or `-Og` for debugging. This gives a 3x speedup.

> [!example] (Parallelization)
> Now we have the `i, k, j` order. Note that the `i` and `j` loops can be parallelized since they don't write to the same data (i.e. `C[i][j]`). We can do this by replacing `for` with `cilk_for`. Due to overhead of parallelization, only parallelizing the outer `i` loop is best (intuitively pleasing as well). This gives almost 18x speedup on our 18 cores.

> [!example] (Divide and conquer)
> The L1D cache can hold 1 row of a single matrix (32 KB), while the L2 cache can hold ~8 rows (256 KB). To get things to fit better in the cache, we can divide-and–conquer the matrix (by splitting into 4 submatrices). This can 2x the performance.

> [!example] (Vector hardware)
> This is SIMD: we have vector registers and vector ALUs. The compiler should be able to do this by default, though it will be conservative. There are more vectorization flags for modern hardware. Note that since floating-point arithmetic is not associative, you might need `–ffast-math` to capture the effect. This can 2x the performance.

> [!example] (AVX intrinsics)
> You can use instructions provided by Intel but won't be produced by the compiler. This can 2x the performance.

Our final algorithm is competitive with Intel's math kernel library! It reaches 41% of the peak performance.

---

**Next:** [[Bit Hacks]]