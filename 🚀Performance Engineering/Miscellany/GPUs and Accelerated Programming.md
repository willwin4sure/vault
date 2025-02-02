> A GPU is a multi-core processor optimized for throughput.

Today, computer hardware is amazing.

A training run for a small modern LLAMA model takes around
$$
\text{1B parameters} \cdot \text{30B tokens} \cdot \text{6 FLOPS/parameter} = 180 \text{ExaFlops}
$$
of computation, yet it can run in a single day on 8 GPUs. Only 50 years ago, the Cray 1 supercomputer would have taken 35,650 *years* to run this computation, and it would be $10^{9}$ times less power efficient.

## Matrix Multiplication on an M2

The peak performance of all the hardware on a standard Apple M2 chip is 20 TeraFlops.

If you use all the tools you've learned in this class about [[Matrix Multiplication|multiplying matrices]], you can get 200 GigaFlops of performance (20,000 times faster than the naive Python implementation). This includes using compiler flags, hand tuning, vectorization, cache blocking, and parallelism.

However, this is just 1% of the chip's total ability. To go beyond this, we have to use the other hardware on the machine. We can go 8 times faster using Apple's `Accelerate` library, by quantizing to `bfloat16` and using their custom matrix units; this gives 1.5 TeraFlops.

If we write thousands of lines of custom Metal compute shaders, we can get 3.2 TeraFlops of performance. Finally, using Apple's specialized neural engine, we can get 16 TeraFlops. In summary:

![[matrix_multiply_apple_m2.png|center|384]]

To understand what's going on, we need to look at the chip die:

![[apple_m2.png|center|384]]

Here are some annotations of the types of hardware we've tried using:

![[annotated_apple_m2.png|center|512]]

We have to write code for all this hardware in order to achieve peak performance! If we want to scale even further and target things like NVIDIA H100s, we need to write even more specialized code.

> [!idea]
> Hardware is amazing, but the gap between *reasonable code* and *peak performance* is enormous.

Why does this gap exist? Well, hardware has changed drastically but software has not kept up. In 1975, we already had: UNIX, C, virtual memory, and decades of work on optimizing compilers.

The issue is that our programming models are a bit outdated. In particular, we usually assume:

* **Sequential and scalar** primitive computation.
* **Uniform and global** heap memory.
* Abstraction through **subroutines** and **pointers**.

We try to expose an abstraction that modern processors are simply old ones, but faster. However, to unlock the true potential of new hardware, we need to rethink how we program.

## Understanding Hardware

> [!question]
> What makes hardware fast?

### Classic Perspective

Fast hardware executes sequential streams of instructions in as little time as possible. Classically, there are a few optimizations here:

1. Increase the clock speed, e.g. via:
	* Higher voltage, but power grows with $v^{2}$.
	* Faster transistors, but this leads to higher leakage.
	* Deeper pipelining, but causes more overhead on mispredictions.

2. Execute multiple instructions per cycle, i.e. superscalar execution, even for single cores.
3. Avoid stalls, e.g. via caching, speculative execution, and sophisticated branch predictors.

### Modern Perspective

Now, even the most advanced processor cores are tiny, and tons can fit on a silicon chip. Further:

> [!idea]
> We most need performance *when we are processing lots of stuff*, in which case we should have abundant data-parallelism!

This leads to a ==throughput-oriented== view of performance, where our goal is to just maximize the aggregate rate of processing many items.

In embarrassingly parallel applications, usually we get rather good linear scaling by just *duplicating chips* when presented with more silicon. Notably, this is *better* than the sublinear scaling we tend to get by applying the classical single-core performance improvement techniques:

![[scaling_single_core_silicon.png|center|512]]

In particular, modern processors are 3x off the trend of just taking your old processors and tiling them.

> [!idea]
> Go backwards are remove all the fancy hardware to optimize single-thread performance. Then, invest this silicon on parallelism.

Take the following fancy core and remove all the stuff on the right.

![[fancy_core.png|center|384]]

Then copy the basic core on the left multiple times.

> [!idea]
> Amortize control overhead with SIMD execution: share the fetch/decode logic.

Note that SIMD can be explicit in your ISA, e.g. Intel AVX or ARM NEON, or implicit in hardware, e.g. in many modern GPUs.

Unfortunately, SIMD requires coherent control, so we have to do special things to handle control flow. We can do this by actually executing both paths of flow but masking out lanes that aren't involved in each branch path.

![[simd_masking.png|center|512]]

At some point in scaling, you'd rather do multicore than add more lanes. This way they're free to diverge and do different things. A typical SIMD width is 8 to 64.

> [!idea]
> Interleave parallel tasks to hide latency.

This solves the problem of dealing with long-latency operations without stalling. Within a single core, it will dedicate itself to many tasks, and it can swap between them during stalls.

![[interleaving_tasks.png|center|384]]

This is analogous to what the OS does when it switches between threads when they block. This can hide hundred cycle latencies to memory as well as few cycle latencies from evaluating branches! And we don't need all that fancy hardware anymore!

However, we need to be able to quickly swap between these tasks! It also requires keeping more state storage around. We do this inside a ==thread context storage==. For an NVIDIA H100, there is storage for 16,000 32-bit register values per warp scheduler (core)! This is 32MB of register storage per GPU, which is comparable to how much a big CPU spends on caching.

If we have many small contexts, this allows us to hide more latency.

> [!idea]
> Amortize data-supply with more complex instructions., e.g. matrix block multiply accumulate instructions.

One issue not yet discussed in energy usage. Here are the energies to move words across levels of the memory hierarchy, as well as the cost of actually performing a floating-point operation.

![[energy_of_data_movement.png|center|384]]

A lot of this just comes from the fact that on-chip data movement costs around 3.2 picojoules to move a word one millimeter.

A few insights to note:

* Compute energy costs of lower-precision arithmetic units are much smaller, which is why model quantization is great.
* Data movement energy costs largely dominate any compute energy costs!

In particular:

> [!claim]
> Data supply dwarfs computation for primitive ALU operations.

Just moving the data *from the register file* totally dominates the energy consumption of performing the operation. To be efficient, we need to reduce data supply cost.

By using complex instructions, the energy overhead of moving data is amortized. In general, we want to seek this kind of arithmetic intensity.