In modern times, transistor counts are still rising but clock speeds have flattened out sharply. One solution is packing multiple cores on a single processor.

The first commercial multicore chip is called Niagara 1, invented by Kunle Olukoton of Stanford. Nowadays, every computer has a multicore chip!

## Multicore Architecture

Here is an abstraction of a ==chip multiprocessor==:

![[chip_multiprocessor.png|center|512]]

There are a bunch of individual processors with individual caches. They are connected by a network to I/O and shared memory.

## Shared-Memory Hardware

> [!idea]
> We use shared-memory as a *persistent medium* for communication between our processors.

Suppose we have some variable `x=3` in memory. If one processor executes a load, it gets pulled down the memory bus and then placed into cache and the registers. When another processor executes the same load, it be pulled from another processor's cache instead (much faster!).

Now, suppose that one processor issues a store `x=5`. We must preserve ==cache coherence==: if other processors already have `x=3` cached, then it won't get the update when it issues a load again!

> [!example] (MSI protocol)
> Also discussed in 6.191, this is a cache coherence protocol that guarantees correctness. Each cache line is labeled with a state:
> 
> * **M**: cache block has been *modified*, and no other caches contain this block in M or S states.
> * **S**: other caches may be *sharing* this block.
> * **I**: this cache block is *invalid* (equivalent to being not there).
>

> [!idea]
> The core idea of MSI is that before you modify a value, you invalidate all other copies of it.

This protocol is not very performant, however, e.g. in the case where all processors are incrementing some shared counter. We call this an ==invalidation storm==. 

## Concurrency Platforms

Programming directly on processor cores is painful and error-prone. ==Concurrency platforms== abstract away synchronization, communication, and load balancing from the programmer. 

We will go through the following platforms:

* pthreads and WinAPI threads.
* Threading Building Blocks (TBB).
* OpenMP.
* Cilk (used in this class!).

Let's parallelize this following Fibonacci program!

```c
#include <inttypes.h>
#include <stdio.h>
#include <stdlib.h>

int64_t fib(int64_t n) {
	if (n < 2) return n;

	int64_t x = fib(n-1);
	int64_t y = fib(n-2);
	
	return x + y;
}

int main(int argc, char* argv[]) {
	int64_t n = atoi(argv[1]);
	int64_t result = fib(n);
	
	printf("Fibonacci of %" PRId64 " is %" PRId64 ".\n", n, result);
	return 0;
}
```

> [!warning]
> Obviously we should not be computing Fibonacci this way, but it is a good exercise in parallelism, representative of other recursive algorithms. We are interested in how our concurrency platforms can handle this execution.

Here are my notes on each concurrency platform: ^f3db33

1. [[pthreads]]
2. [[Threading Building Blocks]]
3. [[OpenMP]]
4. [[Cilk]]

---

**Next:** [[C to Assembly]]