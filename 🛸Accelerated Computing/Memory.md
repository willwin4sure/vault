Recall the [[GPUs and Accelerated Programming]] guest lecture from Performance Engineering; those encompass the first two lectures of the Fall 2024 version of this class. We continue on from Lecture 3.

## GPU Architecture

Let's start looking at some NVIDIA hardware! We'll be looking at the NVIDIA RTX A4000, which uses the Ampere architecture.

### Warp Schedulers

Here is one of the "cores" of the GPU. In NVIDIA terminology, this is all the hardware associated with a single ==warp scheduler==.

![[warp_scheduler.png|center|384]]

It has a bunch of execution units that perform calculations, i.e. 32 x 32-bit execution units. Half of them can also be applied for integer math.

> [!warning]
> For marketing reasons, NVIDIA brags about their hardware by calling each *execution unit* a ==CUDA core==. But they aren't separate "cores" in the normal sense, and it might be better to think of them as vector lanes in a single core.

In particular, note that only 1 warp instruction can be issued across all 32 lanes in a single clock cycle. They all execute in lock-step.

> [!warning]
> Similarly, when writing CUDA, there is a notion of ==CUDA threads== (e.g. when you write `threadIdx.x`. From the software perspective, your minimal unit of parallelism is 32 consecutive CUDA threads that run on your 32 CUDA cores in a ==CUDA warp==.
> 
> But maybe you should think of the entire warp as a "thread" in the conventional sense, since they operate in lock-step on a single instruction stream.

Here is a summary of the somewhat confusing terminology:

| **GPU concept** | **CPU concept** |
| --------------- | --------------- |
| CUDA thread     | Vector lane     |
| Warp            | Thread          |

> [!remark]- Answers to some questions
> In recent generations, they have added separate program counters per lane. This is not for the purpose of executing different instructions per cycle; rather, it allows the code to more easily rejoin divergent branches (e.g. in a nested if-else) for efficiency.
> 
> The warp scheduler can hold 12 warps at a time that it swaps between to hide latency. Note however that there can only be 32 warps in a single block, so unless you have multiple blocks on the same SM (see the next section) you're actually limited to 8. 

On the memory side, there are 512 x 32 x 32-bit registers in one of these warp schedulers. This is basically 512 wide vector registers.

### Streaming Multiprocessors

Every ==streaming multiprocessor== is a cluster of four warp schedulers.

![[streaming_multiprocessor.png|center|512]]

These share some common resources, e.g. a 128KB L1 data cache. This can be used as a conventional cache *or* as a directly-addressed ==scratchpad memory==.

So the cache isn't just used implicitly, where it simply speeds up accesses to some memory locations. You can also have an explicit range of addresses that directly maps to this local array of memory separate from DRAM, and move data in and out of it.

### Whole GPU

The entire GPU consists of 48 SMs. This means altogether we have 192 cores, or over 6000 CUDA cores. Clocked at 1.56 GHz, this delivers **19.1 TFLOPS** of peak compute throughput. There is an extra factor of 2 due to counting each single-cycle fused-multiply-add instruction as 2 floating-point operations (this is standard industry practice when reporting performance).

On the memory side, there are 96k x 1024-bit registers, for a combined **12 MB** of register capacity. The entire chip also holds a **4 MB** L2 cache. There is way more register storage than last-level cache.

We have massive register storage due to the context-switching latency hiding. The L2 cache is smaller because in throughput-processing we're less concerned about *reuse* of data, but rather blasting through it (high-bandwidth streaming). The role of the cache is to aggregate many fine-grained memory operations into big coarse-grained memory operations to access the off-chip memory. It's not sized for reuse but rather to allow a lot of operations to go through.

Do note that the latest generations of GPUs are pushing the size of the last-level cache larger.

## CPU Architecture

As comparison, let's look back at some AMD hardware. We've been using the AMD Ryzen 7 7700 from the Zen 4 generation.

TODO