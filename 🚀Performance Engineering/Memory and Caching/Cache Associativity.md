A review of some 6.191 material on the design of caches.

> [!warning] (Distinction between cache block and line)
> A ==cache block== refers to the *data* that is being moved around, while a ==cache line== refers to the hardware that stores the data. So cache blocks are held within cache lines.

## Fully-Associative Cache

Here is a picture of a toy fully-associative cache. A cache block can be held in any cache line.

![[fully_associative_cache.png|center|512]]

> [!definition] (Tag and offset)
> Every virtual address is divided into the first $w-\log \mathcal{B}$ bits, which form the ==tag==, and the last $\log \mathcal{B}$ bits, which form the ==offset==. Every cache line needs to also store the tag of the resident cache block.

If you want to access a memory location, you have to check the entire cache. The hardware has a parallel lookup (associative memory) that makes this reasonably fast, though it is more expensive.

> [!definition] (Replacement policy)
> If all cache lines are full, you need to ==evict== a block to make room. The order in which you do this is specified by the ==replacement policy==, e.g. LRU, or random.

## Direct-Mapped Cache

Here is a picture of a toy direct-mapped cache. Each cache block can only be found in a specific cache line.

![[direct_mapped_cache.png|center|512]]

> [!definition] (Set)
> The cache blocks are partitioned into ==sets== based on which cache line they map to.

Now, if you want to access a memory location, you only have to check a single line. This is faster and requires less hardware.

Note that the tag can also be shorter, needing only $w-\log \mathcal{M}$ bits, as the next $\log \mathcal{M} - \log \mathcal{B}$ bits specify the set in order to index into the cache.

> [!idea]
> This is particularly good for spatial locality, since any contiguous region will get mapped across the entire cache.

However, if you happen to access a lot of locations with the same tag bits, you will get a ton of [[Memory-Level Parallelism#^dbd02e|conflict misses]]!

## Set-Associative Caches

This is a nice middle-ground. Pictured is a $k$-way set-associative cache.

![[set_associative_cache.png|center|512]]

Now each set can get mapped to any of $k$ cache lines in the cache. Therefore, a direct-mapped cache is just a $1$-way set associative cache, while a fully-associative cache is a $\frac{\mathcal{M}}{\mathcal{B}}$-way set associative cache.

> [!warning]
> One challenge is how to support non-powers-of-2 in your set associative caches (recall that [[Cache-Efficient Algorithms#^69f253|Xeon platinum]] has an 11-way associative L3 cache).

---

**Return:** [[Cache-Efficient Algorithms#^853326]]

