> [Massively-Parallel Mandelbrot](https://accelerated-computing.academy/fall25/labs/lab2/#cta)

## Notes

You should unroll along `i` for the best speedup. This processes local chunks of pixels using the same control flow, minimizing divergence. You can squeeze out a little more performance for the CPU case by changing your vectorization to be over 4x4 blocks (rather than 1x16 rows). Then you can unroll to process 2x2 tiles at once.

CUDA exposes a two-level hierarchy of software parallelism. When you launch a kernel with `<<<..., ...>>>` notation, you are specifying the number of ==blocks== and then the number of CUDA threads per block. A block is guaranteed to run on the same [[Memory#Streaming Multiprocessors|SM]], which consists of 4 warp schedulers. So, modulo hyperthreading, you can saturate our GPU with launch parameters `<<<48, 4 * 32>>>`: you spawn one block per each of our 48 SMs, and then spawn 32 CUDA threads per each of the 4 warp schedulers in an SM.

Some more details about the programming: `threadIdx.x` is unique per thread in a block, and resets across blocks. `blockIdx.x` accesses the index of the block you are in. You can get the number of blocks with `gridDim.x` and the number of CUDA threads per block with `blockDim.x`.