> [SIMD Mandelbrot](https://accelerated-computing.academy/fall25/labs/lab1/)

## Notes

The vectorized CPU implementation is rather painful and requires manually keeping track of the active lanes using a `_mmask16` and using various intrinsic AVX-512 instructions. This is because the compiler cannot vectorize through the innermost `while` loop.

By contrast, the GPU implementation is rather straightforward. The programmer actually specifies the behavior of each *individual* CUDA thread (which is analogous to a vector lane), and all vectorized instructions are handled automatically, including details such as masking.

This does make me wonder how the hardware behaves if you perform some "weird" manipulations to the `threadIdx.x` variable so that each warp processes non-contiguous memory. Maybe the computation proceeds as specified but you lose all speedup.

CPU scalar -> GPU scalar is ~6x slowdown. Around 2.5x is explained by clock speed differences.

The other 2.5x is from architectural differences (superscalar, out-of-order execution, branch prediction).