[[Cache-Efficient Algorithms#Divide and Conquer Method|Last]], we saw an example of a cache-oblivious algorithm, which remarkably uses the cache asymptotically optimally without any cache parameters.

## Heat Diffusion

Today we'll look at an example of simulating heat diffusion, which solves for the heat function $u(t,x,y)$ under the PDE
$$
\frac{ \partial u }{ \partial t } = \alpha \left( \frac{ \partial^{2}u }{ \partial x^{2} } +\frac{ \partial^{2}u }{ \partial y^{2} }  \right),
$$
where $\alpha$ is the ==thermal diffusivity==. Simulating these kinds of problems quickly can be critical for scientific discovery.

In order to simulate this, we must discretize both space and time to get the discrete derivative and Laplacian (something you've seen in 18.704). In one dimension, this would give
$$
\frac{u(t+\Delta t,x)-u(t,x)}{\Delta t}=\alpha\cdot\frac{u(t,x+\Delta x)-2u(t,x)+u(t,x-\Delta x)}{(\Delta x)^{2}}.
$$
This gives us the update rule

```c
u[t+1][x] = u[t][x] + ALPHA * (u[t][x+1] - 2 * u[t][x] + u[t][x-1]);
```

This is called a 3-point stencil.

> [!definition] (Stencil computation)
> A ==stencil computation== updates each point in an array by a fixed pattern, called a ==stencil==. In this case, we won't update the boundaries; we'll hold them fixed (you could also consider the case of a cyclic ring).

Conceptually, we can think of this algorithm in iteration space, a spacetime grid:

![[iteration_space.png|center|384]]

The orange points are the boundary conditions, and the top row is our desired result. The red arrows show the update rule for this computation.

## Naive Solution

Here's some code to do this computation:

```c
double u[2][N];  // "even-odd trick"

static inline double kernel(double* w) {
	return w[0] + ALPHA * (w[-1] - 2 * w[0] + w[1]);
}

for (size_t t = 0; t < T; ++t) {
	for (size_t x = 1; x < N-1; ++x) {
		u[(t+1)%2][x] = kernel( &u[t%2][x] );
	}
}
```

This always keeps one array for the current row and the next row, and flip-flops between them to do the updates.

## Cache-Oblivious Stencil Computation

We will use the [[Cache-Efficient Algorithms#Ideal-Cache Model|ideal-cache model]] again. As before, we care about two performance measures: the work $T_{1}$ and the number of cache misses $Q$.

The naive computation already is relatively nice for the cache since it walks through contiguous memory in order. However, we can do better. Focus on the case where the array is too big to fit in cache, i.e. $n>\mathcal{M}$ and assume an LRU policy.

> [!claim]
> The naive implementation will have a cache miss every $\mathcal{B}$ entries, so $Q=\Theta(NT/\mathcal{B})$. 

How can we do better?

> [!idea]
> Traverse trapezoidal regions in iteration space. You can compute anything inside the trapezoid using only its base.

Here is how we will recurse. Define the width of a trapezoid to be the average of its two bases.

TODO: not necessarily trapezoids??

### Case 1: Squat Trapezoid

This is when the width is at least twice the height. Use a ==space cut== as follows. First compute the trapezoid on the left, then the parallelogram on the right (which depends on the trapezoid on the left).

![[squat_trapezoid.png|center|512]]

Crazily enough, you don't need any other bookkeeping than `u[0]` and `u[1]` (which will have different elements advanced differently in time), since at the boundary you will always have two levels deep on the left, which is enough to compute the right.

### Case 2: Tall Trapezoid 

This is when the width is less than twice the height. Use a ==time cut== as follows. First compute the bottom, then the top.

![[tall_trapezoid.png|center|512]]

The base case is when the height is 1 (or when sufficiently small, you probably want to coarsen). 

### Implementation

```c
void trapezoid(int64_t t0, int64_t t1,
			   int64_t x0, int64_t dx0
			   TODO
			   )
```


### Work and Cache Analysis

The bottom of the trapezoids at the leaves will just fit in cache, so $w=\Theta(\mathcal{M})$. Since all trapezoids have similar widths and heights, the number of points inside will be $\Theta(hw)=\Theta(w^{2})$ and therefore $\Theta(w^{2})$ work. Further, once the leaf can fit in cache, it will only incur $\Theta(w/\mathcal{B})$ cache misses to pull it in and operate on.

In total, there are $\Theta(NT/w^{2})$ leaves and internal nodes. The internal nodes contribute negligibly to work and cache misses. So by just considering the leaves:

> [!claim]
> The work of this algorithm is $\Theta(NT)$ while the number of cache misses is $Q=\Theta(NT/\mathcal{B}\mathcal{M})$. 

This means we're getting temporal locality now!

## Real Code Comparison

Here's an image depicting the two algorithms we've seen at work, under the same simulated environment. The purple dots indicate cache misses, and the cyan dots indicate completed work. The right hand side also depicts the cuts we've used in the divide-and-conquer approach.

![[stencil_comparison.png|center|512]]

Notice how it has progressed much further in the same amount of time!

The lecturer, Charles E. Leiserson, also prepared a heat diffusion simulation in 2D that uses these two algorithms.

* The naive method can sustain 5780 FPS.
* The cache-oblivious method can only sustain 5700 FPS!

> [!question]
> Why??

There are other ingredients to performance! The hardware architecture is probably better at pre-fetching and vectorizing the naive algorithm.

It turns out that in this case, the memory bandwidth into the core suffices for the computations that a single core can run. However, what if we use all the cores on the chip by running a parallel version?

## Parallel Cache-Oblivious Stencil Computation

Time cuts don't parallelize.

Space cuts can be made parallel by allowing so-called upright trapezoids:

![[upright_trapezoid.png|center|384]]

Then, the two sides can be run in parallel before the middle upright trapezoid. 

Now, for the performance:

* The parallel naive method can sustain 10000 FPS. 
* The parallel cache-oblivious method can sustain 25000 FPS.

Now there is a very substantial improvement in performance! This is because memory bandwidth becomes a bottleneck once we run all the cores on the chip.

---

**Return:** [[⛺Memory and Caching Homepage]] 