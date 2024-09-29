> How to optimize work.

> [!definition]
> The ==work== of a program on a given input is the sum total of all the operations executed by the program.

This lecture gives a sampling of various ideas to reduce the amount of work a program must do. This is just a proxy for performance, since it ignores instruction-level parallelism, caching, vectorization, speculation and branch prediction, etc. But it is still a good heuristic.

## Data Structures

Data structures are one of the most important initial choices when writing a program, since they often can be a pain to change.

### Packing and Encoding

> Storing more than one data value per machine word. Converting to a data representation that requires fewer bits.

Here is an example of packing using bit fields which results in 22 bits per date.

```c
typedef struct {
	int year: 13;
	int month: 4;
	int day: 5;
} date_t;
```

### Augmentation

> Adding information to a data structure to make common operations do less work.

For example, adding a pointer to the last element of a singly linked-list.
### Caching

> Store recent results.

### Precomputation and Compile-Time Initialization

> Perform calculations in advance to avoid doing them inside the hot path.

One example is precomputing binomial coefficients via Pascal's triangle. This can be initialized at the start of the program.

However, when humans run programs, they are highly sensitive to the amount of time they have to wait before getting feedback. Another option is to initialize at compile time. This can be done in C via ==metaprogramming==, by writing code to generate the table to paste into your code.

### Sparsity

> Avoid storing and computing on zeros.

Suppose we have a sparse matrix for matrix-vector multiplication. One common representation is called the ==compressed sparse row== representation.

This has three arrays: `rows`, which indexes into `cols` and `vals` to give the breakpoints for the start of the rows. Meanwhile, a single `(col, val)` pair gives the index and value of each nonzero entry in that row.

```c
typedef struct {
	int n, nnz;
	int* rows;     // length n
	int* cols;     // length nnz
	double* vals;  // length nnz
} sparse_matrix_t;

void spmv(sparse_matrix_t* A, double* x, double* y) {
	for (int i = 0; i < A->n; ++i) {
		y[i] = 0;
		for (int k = A->rows[i]; k < A->rows[i+1]; ++k) {
			int j = A->cols[k];
			y[i] += A->vals[k] * x[j];
		}
	}
}
```

Another example is storing a static sparse graph. This is essentially an adjacency list representation but packing all the lists into a single long list and keeping track of the breakpoint indices per vertex.

## Logic

Many of the things in this section can be done by compilers. However, sometimes compilers are finnicky since they must prove that the code behaves equivalently on every input.

### Constant Folding and Propagation

> You can evaluate and substitute constant expressions during compilation. Tag things with `const` to help out the compiler.

### Common Subexpression Elimination

> Avoid computing the same expression multiple times.

Sometimes this cannot be done since it is hard to prove that the values in the expression haven't changed.

### Algebraic Identities

> Replace expensive algebraic expressions with equivalents that take less work.

For example, in collision detection between spheres, instead of asking if $\sqrt{ u }\leq v$, check if $u\leq v^{2}$. There are floating point precision differences, so the compiler might not make these optimizations!

### Creating a Fast Path

> Handle commonly occurring cases faster!

Continuing the collision detection example, you can check bounding boxes first.

### Short-Circuiting

> Stop evaluating once you know the answer.

For example, maybe you have a function that returns if the sum of an array of nonnegative integers exceeds some threshold. You can return as soon as your sum goes over.

### Ordering Tests

> Perform a logical test that is more often successful first before the alternatives.

For example, you might prefer the code

```c
c == ' ' || c == '\n' || c == '\t' || c == '\r'
```

over some other order.
### Combining Tests

> Replace a sequence of tests with one test or switch.

This avoids having multiple branches in sequence. For example, if you have three boolean values `a`, `b`, and `c` and need to bifurcate on each in sequence, you can instead compute a single value `(a << 2) | (b << 1) | c` and then running a `switch` statement on it.

## Loops

Loops are very important since usually the hot path runs through them. Most of the work happens in loops! If we didn't have loops, huge programs would only run for seconds!

### Loop Unrolling

> Minimize control checks and branching by combining several iterations of a loop into a single iteration of the loop.

The tradeoff, of course, is fitting inside the instruction cache. Also, it might be harder if you don't know the number of loop iterations to run.

There is also hierarchical loop unrolling.

### Hoisting

> Move loop-invariant code outside of the loop.

### Sentinels

> Special dummy values in a data structure to simplify logic of boundary conditions.

Might not even be helpful in recent code since branch predictors are good.

### Loop Fusion

> Combine multiple loops over the same index range into a single loop body.

Saves the overhead of loop control.

### Eliminate Wasted Iterations

> Modify loop bounds to avoid executing loop iterations over skipped loop bodies.

## Functions

Functions are vital abstractions but require overhead (e.g. with the stack) to call.

### Inlining

> Replace a call to a function with the body of the function itself.

You can guide the compiler to do this by adding `inline` or similar attributes to the start of functions. Inlining also allows further optimizations using the resulting code.

### Tail Recursion Elimination

> Removes the overhead of a recursive call with a branch.

If the last thing you do is call yourself, instead just go back to the top and reuse the current stack frame with different arguments.

### Coarsening Recursion

> Increase the size of the base case and handle it with more efficient code!