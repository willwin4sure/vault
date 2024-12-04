Recall the [[Storage Allocation#Mark-and-Sweep|mark-and-sweep]] garbage collection algorithm. In the sweep phase, we walk linearly through the allocated memory, striding based on header sizes, and push unmarked addresses onto the free list:

![[sweep_phase.png|center|512]]

Since objects are usually small, we touch most cache lines, so the [[Memory-Level Parallelism#^86befa|L2 prefetcher]] is encouraged to bring data into L2. However, the stride is not constant! Here is the performance:

![[garbage_collection_perf.png|center|512]]

## L1 Stride Prefetcher

This is the [[Memory-Level Parallelism#^ae67b5|prefetcher]] that brings things into L1. Here is the architecture:

![[stride_prefetcher.png|center|512]]

It has a table indexed by instruction pointer. Currently, it holds the last two addresses accessed by that instruction. Then, when a new address comes in, it will examine if they are in an arithmetic sequence (i.e. same stride). If so, it will prefetch ahead in $k$ steps of that stride. Prefetches land in L1 and consume an L1 LFB.

> [!warning] (Why the garbage collector is slow)
> The strides aren't the same, so the L1 prefetcher fails to give you performance improvement.

## Sweep Phase Speedup

The solution is to use a software prefetch. These are enabled by the instruction

```c
__builtin_prefetch(void* addr, int rw, int locality);
```

This fetches the cache line pointed to by `addr` with

* Read/write intention `rw` (`0` is read and `1` is write, default `0`).
* Temporal locality `locality` (`0` lowest to `3` highest, default `3`).

Software prefetches do not actually bind to a register, so it can retire even if the data has not yet arrived, unlike a regular load. It doesn't clog up the out-of-order execution engine. You can keep firing them off. Another nice property is that if you issue two prefetches in close succession that hit the same cache line, they will coalesce and only use up one LFB.

So, to optimize the sweep phase, just add an extra step to software prefetch at some constant distance ahead of the object. Here is the performance gain:

![[prefetching_sweep_phase.png|center|512]]

Now we're actually reaching the upper limits of what you'd expect from a constant stride access pattern.

## Mark-Phase

Jane Street actually uses a depth-first search, but we'll look at how to prefetch a breadth-first search. Basically, you prefetch some fixed distance into the FIFO queue you build for BFS.

---

**Next:** [[Bulk Prefetch]]