The following memories are exposed by the GPU architecture:

![[gpu_mem_hierarchy.png|center|512]]

### Registers

These are small and private to each thread. There are at most 1024 threads per block, so 256 KB makes sense.

### L1/Shared Memory (SMEM)

Every SM has a fast, on-chip scratchpad memory that can be used as an L1 cache and shared memory. All threads in a block can share shared memory, and all CUDA blocks running on a given SM can share the physical memory resource. This memory is why you should have threads in the same block working on localized data for efficiency. For example, if you take the [[Introduction#GPU Optimization|introduction example]] and flip the indexing in the kernel code to

```cpp
int index = threadIdx.x * gridDim.x + blockIdx.x;
```

the runtime quadruples.

### Read-Only Memory

This section is read-only to kernel code, and consists of an instruction cache, constant memory, texture memory (for graphics, I guess), and read-only cache.

### L2 Cache and Global Memory

This cache is shared across all SMs, so every thread in every block can access this memory. This is the cache for the global memory implemented using DRAM.


