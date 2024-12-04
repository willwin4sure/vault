## Matrix Multiplication

Here is the naive way of parallelizing the algorithm:`

```c
cilk_for (int i = 0; i < n; ++i) {
	cilk_for (int j = 0; j < n; ++j) {
		for (int k = 0; k < n; ++k) {
			C[i][j] += A[i][k] * B[k][j];
		}
	}
}
```

We can't parallelize the inner loop since it would cause a ==determinacy race== where the order of the executions affects the answer.

The work is $\Theta(n^{3})$ while the span is $\Theta(n)$, so the parallelism is $\Theta(n^{2})$. When $n=1000$, the parallelism is $10^{6}$, which is plentiful.

As we've discussed before, divide-and-conquer utilizes the cache more efficiently. This splits each matrix into four submatrices, and then it requires

* 8 multiplications of $\frac{n}{2}\times \frac{n}{2}$ matrices.
* 1 addition of $n\times n$ matrices.

With Strassen's algorithm, we could reduce the 8 multiplications down to 7.

How do we represent these submatrices? One way is to just store the top left index and the side length. You also need to be told the full row length in order to look around.

Okay, here's the code.

```c
void mm_dac(double* restrict C, int n_C,
			double* restrict A, int n_A,
			double* restrict B, int n_B,
			int n) {
	assert((n & (-n)) == n);
	if (n <= THRESHOLD) {
		mm_base(C, n_C, A, n_A, B, n_B, n);
	} else {
		double* D = malloc(n * n * sizeof(*D));
		assert(D != NULL);
#define n_D n
#define X(M,row,col) (M + (row*(n_ ## M) + col) * (n/2))
	cilk_scope {
		cilk_spawn mm_dac(X(C,0,0), n_C, X(A,0,0), n_A, X(B,0,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,0,1), n_C, X(A,0,0), n_A, X(B,0,1), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,0), n_C, X(A,1,0), n_A, X(B,0,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,1), n_C, X(A,1,0), n_A, X(B,0,1), n_B, n/2);
		cilk_spawn mm_dac(X(D,0,0), n_D, X(A,0,1), n_A, X(B,1,0), n_B, n/2);
		cilk_spawn mm_dac(X(D,0,1), n_D, X(A,0,1), n_A, X(B,1,1), n_B, n/2);
		cilk_spawn mm_dac(X(D,1,0), n_D, X(A,1,1), n_A, X(B,1,0), n_B, n/2);
		cilk_spawn mm_dac(X(D,1,1), n_D, X(A,1,1), n_A, X(B,1,1), n_B, n/2);
	}
	m_add(C, n_C, D, n_D, n);
	free(D);
	}
}
```

Note the use of the `##` preprocessor paste macro, e.g. if `M` is `C` then `n_ ## M` becomes `n_C`.

```c
void m_add(double* restrict C, int n_C,
		   double* restrict D, int n_D,
		   int n) {
	cilk_for (int i = 0; i < n; ++i) {
		cilk_for (int j = 0; j < n; ++j) {
			C[i * n_C + j] += D[i * n_D + j];
		}
	}		   
}
```

### Reducing Memory Footprint

Can remove the temporary `D` by doing four and then the other four.

```c
void mm_dac(double* restrict C, int n_C,
			double* restrict A, int n_A,
			double* restrict B, int n_B,
			int n) {
	assert((n & (-n)) == n);
	if (n <= THRESHOLD) {
		mm_base(C, n_C, A, n_A, B, n_B, n);
	} else {
#define X(M,row,col) (M + (row*(n_ ## M) + col) * (n/2))
	cilk_scope {
		cilk_spawn mm_dac(X(C,0,0), n_C, X(A,0,0), n_A, X(B,0,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,0,1), n_C, X(A,0,0), n_A, X(B,0,1), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,0), n_C, X(A,1,0), n_A, X(B,0,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,1), n_C, X(A,1,0), n_A, X(B,0,1), n_B, n/2);
	}
	cilk_scope {
		cilk_spawn mm_dac(X(C,0,0), n_C, X(A,0,1), n_A, X(B,1,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,0,1), n_C, X(A,0,1), n_A, X(B,1,1), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,0), n_C, X(A,1,1), n_A, X(B,1,0), n_B, n/2);
		cilk_spawn mm_dac(X(C,1,1), n_C, X(A,1,1), n_A, X(B,1,1), n_B, n/2);
	}
	}
}
```

However, this makes the span $\Theta(n)$ instead of $\Theta(\log n)$. But we have enough parallelism anyway so trading it off for smaller memory footprint is fine.

> [!remark]
> Side note: in the `mm_base` function (implemented with a triple `for` loop), it is often helpful to explicitly tell the compiler the size using an if statement, which seems to allow further optimizations.

## Parallel Merge Sort

```c
void p_merge_sort(int* restrict A, int n, int* restrict B) {
	assert(n > 0);
	if (n == 1) {
		B[0] = A[0]; return;
	}
	int C[n];
	cilk_scope {
		cilk_spawn p_merge_sort(A, n/2, C);
		p_merge_sort(A+n/2, n-n/2, C+n/2);
	}
	merge(C, n/2, C+n/2, n-n/2, B);
}
```

The idea of the parallel merge. We have two sorted arrays `A` and `B`. We take the median of `A` (which we take as the larger one) and binary search for it in `B`: then we recursively parallel merge the two halves. Then, the largest possible subproblem is $\frac{3}{4}$ the size of the original.

```c
void p_merge(int* restrict A, int na,
			 int* restrict B, int nb,
			 int *C) {
	if (na < nb) {
		p_merge(B, nb, A, na, C); return;  // Tail call can be optimized.
	}

	if (na == 0) return;
	int ma = na / 2;
	int mb = binary_search(A[ma], B, nb);
	C[ma + mb] = A[ma];
	cilk_scope {
		cilk_spawn p_merge(A, ma, B, mb, C);
		p_merge(A + ma + 1, na - ma - 1, B + mb, nb - mb, C + ma + mb + 1);
	}
}
```

> [!definition] (Work efficiency)
> Let $T_{S}(n)$ be the running time of the best serial algorithm on a given input of size $n$. Let $T_{1}(n)$ be the work of a parallel program (its running time on $1$ processor) on the same input.
> 
> The ==work overhead== of the parallel program is the worst-case ratio $\lambda(n)=T_{1}(n)/T_{S}(n)$. We say that a parallel program is ==work efficient== if $T_{1}(n)\sim T_{S}(n)$, or equivalently $\lambda(n)\sim 1$. We say that it is ==asymptotically work efficient== if $\lambda(n)=\Theta(1)$.

> [!claim] (Work-first principle)
> Suppose the worst-case work overhead if $\lambda=T_{1}/T_{S}$. The [[DAGs, Work, and Span#^935a5c|work law]] says that
> $$
> T_{p}\geq \frac{T_{1}}{p} = \lambda \cdot \frac{T_{S}}{p}.
> $$
> Therefore, if $\lambda$ is large, we *cannot* get near-perfect linear speedup over the good serial code no matter how many processors we run on!

> [!idea]
> We waste processing power proportional to the work overhead.

It's fine if the span has overhead, as long as we have sufficient parallelism.

---

**Return:** [[⛺Parallelization Homepage]