> Heap memory management techniques.

## Memory Allocation Primitives

The machine has a physical memory and a physical disk, and we'd like to share it across multiple processes that want to remain oblivious to one another. We solve this—like many other problems in CS—by adding a layer of indirection, using [[Virtual Memory|virtual memory]]!

Every process has its own virtual address space, and it gets mapped to the physical memory/disk via the `mmap()` sys call (one other use case is to memory map a file so that multiple processes can communicate): it starts by being lazy and just maps the virtual page to a null page. However, if you now try to access this page in virtual address space, it triggers a page fault, which costs $\sim 10^{6}$ cycles.

The virtual-to-physical address translation uses the following process:

![[virtual_to_physical.png|center|512]]

We use a hierarchical structure since storing the full page table would be prohibitively large; instead, we assume that only small local bits of virtual address space will actually be used, so the tree has large memory savings.

![[tlb_accesses.png|center|512]]

In reality, there also exists a translation lookaside buffer which caches these page walks (as seen in 6.1910). Therefore, the size of your pages greatly influences the maximum size of your performant working sets, since you watn them all to fit in the higher levels of your TLB caches. ^97f27c

## Methods of Heap Allocation

Recall the [[Linux x86-64 Calling Conventions#Program Layout|layout of virtual memory in a program]]. There is a stack that grows downward. However, we have yet to discuss the heap that grows upward. The heap allows us to allocate memory non-locally, so that we can access it from anywhere in the program.

### Manual Management

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

The heap-management code in the C library provides the `malloc()` and `free()` interface for the user, which is responsible for satisfying user requests by reusing freed memory whenever possible. You will implement your own `malloc()` and `free()` in [[Project 3]].

The heap-management code uses the lower-level system facilities, including `mmap()`, in order to obtain memory from the kernel. 

> [!warning]
> Failure to `free` or `delete` creates a memory leak. But watch out for dangling pointers (pointers to freed memory) or double frees (freeing memory that has already been freed).

### Garbage Collection

Some other programming languages like Python have garbage collection, which automatically reference counts objects and reclaims storage that is provably unused. However, this comes at some performance cost due to the overhead.

## Properties of Storage Allocators

> [!definition] (Speed of memory allocators)
> Allocator ==speed== is the number of allocations and deallocations per second that the allocator can sustain.

> [!idea]
> It is more important to maximize allocator speed for small blocks. This is because for large blocks, initialization time should be a small overhead compared to all the operations the user will do on this memory.

> [!definition] (Footprint, fragmentation, utilization)
> * The ==user footprint== is the maximum $M$ of the number of bytes in use by the user program over all time.
> * The ==allocator footprint== is the maximum $H$ of the number of bytes provided to the allocator by the operating system. Usually this grows monotonically over time.
> * The ==fragmentation== is $F=H/M$.
> * The ==space utilization== is $M/H$.

What causes fragmentation?

* ==Space overhead== is space used by the allocator for bookkeeping.
* ==Internal fragmentation== is waste due to allocating larger blocks than the user requests.
* ==External fragmentation== is waste due to the inability to use storage because it is not contiguous.
* ==Blowup== is the additional space a parallel allocator uses beyond what a serial allocator would require.

## Manual Heap Allocation Techniques

Here are some methods for how a heap allocator can be written. The goal is to expose a `malloc/free` interface.

### Fixed-Size Heap Allocation

> [!example] (Bitmap allocator)
> Uses a bitmap to keep track of which blocks of `A` are free and which are used. Block sizes can be arbitrarily small. Some tricks can improve things:
> 
> * **Bit tricks**, e.g. `bitmap & (-bitmap)`.
> * **Architecture-specific intrinsics**, e.g. `_mm_tzcnt` to count trailing zeros.
> * **Multilayer hierarchy** via a bitmap per page and bitmap for pages.

![[bitmap_allocator.png|center|512]]

> [!example] (Free list allocator)
> Every piece of storage has the same block size. Each unused storage block contains a pointer to the next unused block (the block size must be at least the size of the pointer). This singly linked list supports fast lookup for the first free block and fast insertion when a block is freed.

![[free_list_allocator.png|center|512]]

The free list allocator has good temporal locality but poor spatial locality due to external fragmentation: blocks are often distributed across virtual memory which can increase the size of the page table and cause ==disk thrashing==.

One way to mitigate this is to keep a free list per disk page. Then, you can allocate from the free list for the fullest unfull page: this way, pages usually remain full so you get better locality.

### Variable-Size Heap Allocation

> [!example] (Binned free list allocator)
> You have a bunch of free lists indexed by $r$, where free list $r$ has $2^{r}$ block size. If you want to allocate $x$ bytes, then first compute $k=\lceil \log x \rceil$. If that bin is nonempty, return a block. Otherwise, find a block in the next larger nonempty bin $k'>k$, and split it up into blocks of sizes $2^{k'-1},\dots,2^{k},2^{k}$, and give one of the $2^{k}$ away. If no block exists, ask the OS for more memory.

![[binned_free_list_allocator.png|center|512]]

> [!question]
> We effectively never run out of virtual memory, so why not keep allocating increasing virtual addresses and never free?

The external fragmentation would be horrendous! The performance of the page table would degrade tremendously leading to disk thrashing. The goal of storage allocators is to keep the heap compact.

> [!theorem] (BFL has logarithmic fragmentation)
> Suppose the maximum amount of heap memory in use at any time by a program is $M$. If the heap is managed by a BFL allocator, the amount of virtual memory consumed by heap storage is $\mathcal{O}(M \log M)$.

This is a pretty loose bound: keeping a constant factor over $M$ is not surprising to achieve. It turns out that BFL is 6-competitive with the optimal omniscient allocator (assuming no ==coalescing==).

We saw in the BFL allocator that we could break large blocks into small ones. The performance can be improved by also coalescing, i.e. slicing together adjacent small blocks into a larger block!

Clever schemes exist for finding adjacent free blocks efficiently, e.g. the "buddy" system or using footers in addition to headers. There are no good theoretical bounds that prove the effectiveness of coalescing, but it works in practice.

## Parallelization

### Global Heap

One way is to just have a global heap and a big lock on it. This has blowup 1 but is rather slow (acquiring a lock is like an L2 cache access). The contention of the lock also prevents scalability.

### Local Heaps

Another option is to have local heaps, i.e. each thread has its own heap. However, this suffers from ==memory drift==: blocks allocated by one thread being freed on another leads to unbounded blowup. This is because if you do all the allocations in one heap and all the freeing in another heap, if the first thread tries to request more memory it won't see the free space and keep asking the OS for more.

> [!idea]
> One way to implement this is having different large chunks of memory for each heap. You could also just have each hold its own binned free-list allocator, so there's no restriction on where they appear in physical memory.

### Local Ownership

The issue here is that different threads could perform the allocation and deallocation of a block. One way to solve this is to label each block with a thread owner, and return freed objects pack to the owner's heap. This does require synchronization when freeing a remote block, but the blowup is now bounded by the number of processors.

This is also resilient to ==false sharing==, an issue that occurs since memory is accessed in quanta of cache lines. A program may place `x` and `y` on the same cache block and if two processors access each independently, it will still cause the cache block to be ping-ponged back and forth between the cache lines of the processors. Local ownership tends to reduce passive false sharing. ^1861d8

### Hoard Allocation

Now there is one global heap and $p$ local heaps. Memory is organized into large ==superblocks== of size $S$, so the local and global heaps can transact in terms of this large quanta.

This is fast, scalable, has bounded blowup, and is resilient to false sharing. Here's how `malloc()` would work on thread `i`:

```c
if (there exists a free object in heap i) {
	x = an object from the fullest nonfull superblock in heap i;
} else {
	if (the global heap is empty) {
		B = a new superblock from the OS;
	} else {
		B = a superblock in the global heap;
	}
	set the owner of B to i;
	x = a free object in B;
}
```

Hoard allocation will maintain the invariant that if `m_i` is the in-use storage of heap `i` and `h_i` is the storage owned by heap `i`, then
$$
m_{i}\geq \min(h_{i}-2S,h_{i}/2).
$$
Basically, we always want to be wasting at most half the memory we own or at most two superblocks. So here's how `free(x)` would work on thread `i`:

```c
put x back in heap i;
if (m_i < MIN(h_i - 2 * S, h_i / 2)) {
	move a superblock that is at least half empty from heap i
	to the global heap;
}
```

This movement allows the global heap to give the superblock to another local heap that will make better use of it. Any remaining objects on the superblock remain valid.

> [!theorem]
> Let $M$ be the user footprint for a program and let $H$ be the Hoard allocator's footprint. The blowup is bounded by
> $$
> H/M = \mathcal{O}(SP/M+1).
> $$

This is because the unused storage is at most $\mathcal{O}(SP)$ for the $h_{i}-2S$ term plus $\mathcal{O}(M)$ for the $h_{i}/2$ term.

### Other Solutions

* `jemalloc`. Has a separate global lock for each allocation size. Also allocates the object with the smallest address among all objects of the requested size.
* `SuperMalloc`. This is very simple in terms of lines of code, and extremely fast.

## Garbage Collection

The idea of garbage collection is to unburden the programmer from manually freeing objects. Instead, the garbage collector will identify and recycle objects that the program can no longer access. 

> [!definition] (Roots, live, dead)
> * ==Roots== are objects directly accessible by the program (globals, stack, etc.).
> * ==Live== objects are reachable from roots by following pointers.
> * ==Dead== objects are inaccessible and can be recycled.

> [!question]
> How does the GC identify pointers?

This can be done with strong typing. It also often requires prohibitions on pointer arithmetic.

> [!example] (OCaml data constraints)
> In OCaml, every 8-byte piece of data is either a 63-bit integer or a 63-bit pointer. The LSB is reserved as a tag identifying which it is.

### Reference Counting

Like how [[C++ shared pointers]] work. We keep a count of the number of pointers referencing each object, and if the count goes to zero, we free the object.

![[ref_counting.png|center|512]]

However, this has issues if there is a cycle in the graph. One way of resolving this is enforcing that the ownership pointers are always a DAG, and any other pointers are weak.

### Mark-and-Sweep

This just performs a BFS to mark things that are reachable, and then sweeps through the memory deallocating anything that is not. One of your homework assignments is to speed up this process via prefetching (something Hasenplaugh actually worked on at Jane Street!).

### Stop-and-Copy

Now instead of sweeping away all the dead things (which results in external fragmentation), we copy all the data into another space (we could imagine swapping back and forth between two buffers). This results in *no* external fragmentation.

However, the hard part is actually updating all the pointers. The basic idea is:

1. When an object has already been copied to the `TO` space, we can hijack the `FROM` space memory to store a forwarding pointer from the start of the block in `FROM` to the start of the block in `TO`. This also implicitly marks the block as moved.
2. When an object is removed from the BFS queue in `TO` space, update all the pointers through the forwarding pointers.

---

**Next:** [[Memory-Level Parallelism]]