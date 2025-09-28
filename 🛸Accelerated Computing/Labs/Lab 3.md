> [Wave Simulation](https://accelerated-computing.academy/fall25/labs/lab3/)

## Notes

The upshot of this lab is that blocks (software units of computation that run on the same SM) can share scratchpad space in the L1 cache of that SM. Here is the CUDA syntax to achieve this:

```cpp
__global__ void my_kernel(/* ... */) {
	// shared memory across CUDA threads with requested size in bytes
	extern __shared__ float my_shared_memory[];
}

// before launching the kernel, if shmem_size_bytes is > 48KB.
// upper limit for shmem_size_bytes is 100KB.
CUDA_CHECK(cudaFuncSetAttribute(
	my_kernel,
	cudaFuncAttributeMaxDynamicSharedMemorySize,
	shmem_size_bytes));

// later, launching the kernel:
my_kernel<<<grid_dim, block_dim, shmem_size_bytes>>>(/* ... */);
```

Another useful primitive is `__syncthreads()`, which is a barrier between all CUDA threads in a block.