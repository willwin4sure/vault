## Memory Allocation Primitives

The machine has a physical memory and a physical disk, and we'd like to share it across multiple processes that want to remain oblivious to one another. We solve this by adding a layer of indirection, using [[Virtual Memory|virtual memory]]!

Every process has its own virtual address space, and it gets mapped to the physical memory/disk.

The `mmap()` sys call performs memory mapping: it starts by being lazy and just maps it to a null page. However, if you now try to access this page in virtual address space, it triggers a page fault which costs ~$10^{6}$ cycles.

```c
/**
 * Allocate and return a pointer to a block of memory containing
 * at least `s` bytes.
 */
void* malloc(size_t s);

/**
 * `p` is a pointer to a block of memory returned by `malloc()`
 * or `memalign()`. Deallocate the block.
 */
void free(void* p);

/**
 * Allocate and return a pointer to a block of memory containing
 * at least `s` bytes, aligned to a multiple of `a`, where `a`
 * must be an exact power of 2.
 */
void* memalign(size_t a, size_t s);
```

The heap-management code in the C library provides the `malloc()` and `free()` interface for the user, which is responsible for satisfying user requests by reusing freed memory whenever possible.

The heap-management code uses the lower-level system facilities, including `mmap()`, in order to obtain memory from the kernel. 

> [!warning]
> Failure to `free` or `delete` creates a memory leak. But watch out for dangling pointers (pointers to freed memory) or double frees (freeing memory that has already been freed).

Some other programming languages like Python have garbage collection, which automatically reference counts objects and reclaims storage that is provably unused.

> [!definition]
> Allocator ==speed== is the number of allocations and deallocations per second that the allocator can sustain.

> [!idea]
> It is more important to maximize allocator speed for small blocks. This is because for large blocks, initialization time should be a small overhead compared to all the operations the user will do on this memory.

> [!definition]
> * The ==user footprint== is the maximum $M$ of the number of bytes in use by the user program over all time.
> * The ==allocator footprint== is the maximum $H$ of the number of bytes provided to the allocator by the operating system.
> * The ==fragmentation== is $F=H/M$.
> * The ==space utilization== is $M/H$.

What causes fragmentation?

* ==Space overhead== is space used by the allocator for bookkeeping.
* ==Internal fragmentation== is waste due to allocating larger blocks than the user requests.
* ==External fragmentation== is waste due to the inability to use storage because it is not contiguous.
* ==Blowup== is the additional space a parallel allocator uses beyond what a serial allocator would require.

## Fixed-Size Heap Allocation

> [!example] (Bitmap allocator)
> Uses a bitmap to keep track of which blocks of `A` are free and which are used. Block sizes can be arbitrarily small. Some tricks can improve things:
> 
> * **Bit tricks**, e.g. `bitmap & (-bitmap)`.
> * **Architecture-specific intrinsics**, e.g. `_mm_tzcnt` to count trailing zeros.
> * **Multilayer hierarchy** via a bitmap per page and bitmap for pages.

> [!example] (Free list allocator)
> Every piece of storage has the same block size. Each unused storage block contains a pointer to the next unused block (the block size must be at least the size of the pointer). This singly linked list supports fast lookup for the first free block and fast insertion when a block is freed.

The free list allocator has good temporal locality but poor spatial locality due to external fragmentation: blocks are often distributed across virtual memory which can increase the size of the page table and cause ==disk thrashing==.

One way to mitigate this is to keep a free list per disk page. Then, you can allocate from the free list for the fullest unfull page: this way, pages usually remain full so you get better locality.

## Variable-Size Heap Allocation

> [!example] (Binned free list allocator)
> You have a bunch of free lists indexed by $r$, where free list $r$ has $2^{r}$ block size. If you want to allocate $x$ bytes, then first compute $k=\lceil \log x \rceil$. If that bin is nonempty, return a block. Otherwise, find a block in the next larger nonempty bin $k'>k$, and split it up into blocks of sizes $2^{k'-1},\dots,2^{k},2^{k}$, and give one of the $2^{k}$ away. If no block exists, ask the OS for more memory.

> [!question]
> We effectively never run out of virtual memory, so why not keep allocating increasing virtual addresses and never free?

The external fragmentation would be horrendous! The performance of the page table would degrade tremendously leading to disk thrashing. The goal of storage allocators is to keep the heap compact.

> [!theorem]
> Suppose the maximum amount of heap memory in use at any time by a program is $M$. If the heap is managed by a BFL allocator, the amount of virtual memory consumed by heap storage is $\mathcal{O}(M \log M)$.

This is a pretty loose bound: keeping a constant factor over $M$ is not surprising to achieve. In fact, BFL is 6-competitive with the optimal omniscient allocator (assuming no ==coalescing==).

We saw in the BFL allocator that we could break large blocks into small ones. The performance can be improved by also coalescing, i.e. slicing together adjacent small blocks into a larger block!

Clever schemes exist  for finding adjacent blocks efficiently, e.g. the "buddy" system. There are no good theoretical bounds that prove the effectiveness of coalescing, but it works in practice.

## Parallelization

One way is to just have a global heap and a big lock on it. This has blowup 1 but is rather slow (acquiring a lock is like a L3-cache access). The contention of the lock also prevents scalability.

Another option is to have local heaps, i.e. each thread has its own heap. However, this suffers from ==memory drift==: blocks allocated by one thread being freed on another leads to unbounded blowup.

TODO: hoard allocation