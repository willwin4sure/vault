> Exploiting the fact that your computer can perform multiple memory operations simultaneously.
## Cache Architecture

Here is the classic hierarchy of memory in a computer. Memory gets larger and slower and you move down.

![[memory_hierarchy.png|center|512]]

One thing that is particularly interesting is that these access latencies have not really changed over time. This is because latency is governed by the capacitance of metal, which doesn't really improve with technology (on the other hand, transistors do improve over time).

The gap has now widened to the point that there is ~100-1000x difference in performance between a program that fetches all its operands from memory versus from registers.

Intel's Cascade Lake architecture, shown below, is the test system for the various MicroBenchmarks in this lecture. ^ae67b5

![[intel_cascade_lake.png|center|384]]

Notice the presence of [[Storage Allocation#^97f27c|TLBs]] that we discussed last time. There are also these so-called ==line-full buffers== and ==prefetchers==, which we will discuss later this lecture.
 
> [!definition] (Cache locality)
> Programs often access a relatively small portion of the address space at any given time.
> 
> * ==Temporal locality==: if an item is referenced, it will tend to be referenced again soon.
> * ==Spatial locality==: if an item is referenced, nearby items will tend to be referenced soon.

^bcfc95

Caches, the *single greatest computer architectural innovation of the last 60 years*, rely on locality to be effective.

> [!definition] (Cache issues)
> * A ==cold miss== is the first time the data is accessed. Prefetching may reduce the cost.
> * A ==capacity miss== is when the previous access has already been evicted since too much data has been touched in between (more data than size of cache). Can try to reorganize data so reuse occurs before eviction, or prefetch otherwise.
> * A ==conflict miss== is when multiple data items map to the same location so they get evicted. Rearranging/padding arrays may help... this is possible given [[Cache Associativity|associativity]].

^dbd02e

> [!definition] (Cache sharing issues)
> *  A ==true sharing miss== is when multiple processors want to modify the same data and it requires moving across the caches. Can be mitigated by minimizing sharing/locks.
> * A ==false sharing miss== is when multiple processors modify *different* data in the same cache line, but the lines still have to get moved. We talked about these [[Storage Allocation#^1861d8|last lecture]].

## Cycle with Read Next

Let $p$ be a prime such that $2$ is a primitive root of the finite field $\text{GF}(p)$. Then, we initialize the array $A$ with the cycle $A[n]=2n\pmod{p}$ for $n\in[1,p-1]$.

Then, we will try to traverse this array in the following order:

```c
int cycleWithReadNext(int* A, int p, int iterations) {
	int sum = 0;
	int n = 1;
	for (int i = 0; i < iterations; ++i) {
		sum += A[n];
		n = A[n];
	}
	return sum;
}
```

This adds in the order of powers of $2$ modulo $p-1$. Here's what the performance graph looks like:

![[cycle_read_next.png|center|512]]

Note that there is a [[Compiler Vectorization#Loop Carried Dependencies|loop-carried dependence]] with the assignment `n = A[n];` that prevents vectorization of multiple iterations at once. This means there is a strict data dependence through `*A` between each iteration. 

![[performance_array_size.png|center|512]]

We can annotate the graph with the latencies of the memory hierarchy and when they become relevant. 
## Performance Counters

Modern processors can track all sorts of events relevant to improving performance:

* Cycles,
* Cycles spent stalling waiting for memory.
* Cycles stalled waiting for instruction cache.
* Number of loads outstanding.
* Cache misses at each cache level.
* TLB misses.
* Branch mispredictions.
* etc.

We can annotate our previous graph with these performance counters:

![[misses.png|center|512]]

The orange, green, and blue lines are the L1, L2, and L3 cache miss counts. However, another interesting bottleneck for performance is the gray line, which counts TLB misses. On a standard machine with 4KB pages, the [[Storage Allocation#^97f27c|TLB performance working set]] caps out at 6MB before we have to start doing page walks.

## Cycle with Calculate Next

What if instead we calculated the next index instead of reading it off of memory?

```c
int cycleWithCalcNext(int* A, int p, int iterations) {
	int sum = 0;
	int n = 1;
	for (int i = 0; i < iterations; ++i) {
		sum += A[n];
		n = 2 * n;
		n = n > p ? n - p : n;
	}
	return sum;
}
```

Now, the loop-carried dependence goes through a register! Here's the new performance graph, in orange:

![[cycle_calc_next.png|center|512]]

Some interesting characteristics are highlighted. For example:

> [!question] (Interesting characteristics)
> * Why is the L1 performance faster?
> * Why is the L2 performance no longer dependent on L2 latency, and just L1 latency?
> * Why does the L3 performance start so much better then suddenly decay?

> [!idea] (Answer to first question)
> The answer to the first question is because we no longer have a strict dependence of our loop through the L1 memory access which has 1.6 ns of latency.
> 
> Instead, we're just trying to run the instructions as fast as we can in the loop. The loads can be executed in parallel using out-of-order execution to calculate the loop and sending multiple loads simultaneously with the LFB.

> [!claim] (Little's law)
> Let $W$ be the average latency (ns), $\lambda^{-1}$ be the nsPerAccess, and $L$ be the memory-level parallelism. Then,
> $$
> \frac{W}{L}=\lambda^{-1}.
> $$

> [!idea] (Answer to second question)
> It turns out we're just not bottlenecked by memory. We have $L=10$ with our LFBs and $W=5.6\text{ ns}$ using the L2 cache, so it takes $0.56\text{ ns}$ per access. However, the loop itself still takes $1.4\text{ ns}$.
> 

> [!idea] (Answer to third question)
> By the first arrow, the difference in performance is just a factor of 10 (28 ns to 2.8 ns), purely from having 10 LFBs. For the second arrow, however, we have to do page walks for each access. The page walker also operates serially, so it ends up being slow.
> 

## Sequential with Read Next

Now suppose instead that `A` is initialized with a sequential array, so we don't jump around in memory. Then, here's the performance graph:

![[sequential_read_next.png|center|512]]

It is basically invariant to size! However, this can't just be due to the parallelism achieved via cache blocks: if we were doing a memory load every 8 accesses, that would take 15 ns per access as opposed to the 1.6 ns we see.

So what's aiding our performance? Well, it turns out to be the ==L2 stream prefetcher==.

![[stream_prefetcher.png|center|512]]

The stream prefetcher is a state machine that operates on 4KB pages. For each page, it looks for L1 miss streams that are sequential and of sufficient density, and then starts an initial burst of loads and afterward continuously prefetches ahead.

The L2 prefetcher can exploit an entire 24 LFBs. This means that the memory bandwidth bottleneck is around $120/(24 \cdot 8)=0.625\text{ ns}$ now, clearly fast enough for the performance we see. ^86befa

> [!warning]
> One annoying fact is that once you get to the end of the page you stop prefetching. However, this is nontrivial to fix since the prefetcher operates on physical pages, while to continue prefetching you would have to figure out the next virtual page.

## Sequential with Calculate Next

Now, we calculate the next index instead. Removing the loop-carried dependence through L1 cache accesses allows greater instruction-level parallelism and the reduced memory bottleneck truly shines:

![[sequential_calc_next.png|center|512]]

We can perform even better by loop unrolling the inside.

```c
int sequentialWithCalcNextBlocked(int* A, int p, int iterations) {
	int sum = 0;
	int n = 1;
	for (int i = 0; i < (iterations >> 4); ++i) {
		for (int j = 0; j < (1 << 4); ++j) {
			sum += A[n];
			n = n + 1;
			n = n > p ? n - p : n;
		}
	}
	return sum;
}
```

Since the memory was never the bottleneck, the reduced number of instructions helps greatly:

![[sequential_calc_next_blocked.png|center|512]]

The start of the dark blue curve shows a bottleneck from the rate of processing the instruction stream. The end shows the bottleneck from memory fetches.

## Striding Through an Array

When the stride is large, missing the TLB becomes the bottleneck, and it's quite punishing.

![[long_stride.png|center|512]]

When the stride is medium, the main memory access speed becomes the bottleneck. We lose a factor of 8 since we can only use one element per cache block.

![[medium_stride.png|center|512]]

Now, go read about a case study on Jane Street's OCaml garbage collector. There, you will learn about the L1 stride prefetcher. Then, go learn about buik prefetching. ^533543

1. [[Jane Street's OCaml Garbage Collector]]
2. [[Bulk Prefetch]]

---

**Next:** [[Cache-Efficient Algorithms]]