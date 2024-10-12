There are two types of vectorizers that compilers implement:

1. Loop vectorization.
2. Superword Level Parallelism (SLP) vectorization.

> [!definition] (Loop vectorization)
> Whenever you see a loop where each loop body is independent, you can do multiple iterations with a single SIMD vector operation instead.

> [!definition] (SLP vectorization)
> A bit more sophisticated where you unroll a few iteration of the loop, group isomorphic instructions together, and then vectorize what you can.

> [!warning]
> Automatic vectorizers are good, but not ubiquitous! Even some simple code doesn't get vectorized by compilers! There is a lot of performance that can be captured by manual tuning.

## What Makes Vectorization Hard

### Loop with Unknown Trip Count

```c
void bar(float* A, float K, int start, int end) {
    for (int i = start; i < end; ++i)
        A[i] = K;
}
```

The compiler tries to use `zmm` registers to vectorize 16 at a time, then `ymm` registers to vectorize 8 at a time, then it just does things 1 at a time.

### Cannot Prove Independence

```c
void bar(float* A, float* B, float K) {
    for (int i = 0; i < 1024; ++i)
        A[i] = B[i] + K;
}
```

This can't necessarily be vectorized if `A` and `B` point to overlapping sections of memory. So the compiler usually performs a check if the pointers are far apart enough first before vectorizing.

To avoid this check, add `restrict` to the pointer arguments.

### Loop Carried Dependencies

```c
void bar(float* restrict A) {
    for (int i = 0; i < 1024; ++i)
        A[i] += A[i-1];
}
```

Now each iteration of the loop depends on the last. If instead it said `A[i] += A[i-2];`, the compiler will do two iterations at a time.

### Reductions

```c
int foo(int* A, int n) {
	unsigned sum = 0;
	for (int i = 0; i < 1024; ++i)	
		sum += A[i] + 5;
	return sum;
}
```

Lane operations are fast for vector machines, but cross-lane operations are a bit slower. First, it computes partial sums per-lane. Now it just needs to reduce all the lanes together, which it does in a tree-reduce fashion.

### Control Flow

```c
int foo(int* A, int n) {
	unsigned sum = 0;
	for (int i = 0; i < 1024; ++i)
	    if (A[i] > 0)
	        sum += A[i] + 5;
	        
	return sum;
}
```

This does the vectorized comparison masking trick before adding.

### Non-Contiguous Data

```c
void bar(float* restrict A, float* restrict B, float K) {
    for (int i = 0; i < 1024; ++i) {
        B[2*i] = A[i] + K;
        B[2*i+1] = A[i] * K;
    }
}
```

Mask out the even or odd lanes, perform the relevant computations, and then mask them back into the result.

```c
void bar(float* restrict A, float* restrict B, int* restrict X) {
    for (int i = 0; i < 1024; ++i) {
        B[i] = A[X[i]];
    }
}
```

This uses a gather operation.

### Different Hardware Versions

```c
void bar(float* A, float* restrict B) {
    for (int i = 0; i < 1024; ++i)
        A[i] += B[i];
}
```

Based on different architectures, this gets different levels of vector parallelism.

> [!remark]
> Two skipped topics are **cost model failures** and **vectorization of function calls**.

---

**Next:** [[Manual Vectorization Examples]]
