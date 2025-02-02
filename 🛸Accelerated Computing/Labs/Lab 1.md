> [SIMD Mandelbrot](https://accelerated-computing-class.github.io/fall24/labs/lab1/)

## Notes

The vectorized CPU implementation is rather painful and requires manually keeping track of the active lanes using a `_mmask16` and using various intrinsic AVX-512 instructions. This is because the compiler cannot vectorize through the innermost `while` loop.

By contrast, the GPU implementation is rather straightforward. The programmer actually specifies the behavior of each *individual* CUDA thread (which is analogous to a vector lane), and all vectorized instructions are handled automatically, including details such as masking.

This does make me wonder how the hardware behaves if you perform some "weird" manipulations to the `threadIdx.x` variable so that each warp processes non-contiguous memory. Maybe the computation proceeds as specified but you lose all speedup.

## Writeup

1. The runtime of the scalar GPU implementation is 372ms while the runtime of the scalar CPU implementation is 40ms. The GPU implementation is slower since we are unable to exploit any parallelism in the scalar implementations, and are instead just introducing overhead, e.g. by transferring data from device. Also CPU cores should be a lot more powerful than the GPU cores for executing single-threaded programs due to, e.g. out-of-order execution, branch prediction, prefetching, etc.

2. The vectorized CPU implementation is almost 10x faster than the scalar CPU implementation (on AWS EC2, 6.3ms vs 57.3ms). I initialize `cx` in the outer loop to a constant value with `_mm512_set1_ps()` and I initialize `cy` in the inner loop by constructing it out of a fixed `_mm512_set_ps()` outside the loop (in the variable `arange`) and vectorized operations. I handled branching by adding a mask.

3. The vectorized GPU implementation is 20x faster than the scalar GPU implementation, running in around 18ms on my local machine. The GPU probably executes multiple kernel instances that run different number of iterations using a similar masking trick to our vectorized CPU code.