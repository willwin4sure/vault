> CUDA is a parallel computing platform and programming model created by NVIDIA.

Yes, it's the thing you see whenever you run programs on a GPU in PyTorch. You need a CUDA-capable GPU (I'm using an RTX 3080) and the free CUDA toolkit. You'll also need a C++ compiler.

## Adding Numbers

Here is a basic program in C++ that takes two float arrays of length $2^{20}$ and adds them elementwise. It runs in about **1 millisecond** on my 13th Gen Intel Core i7-13700KF processor.

```cpp
/**
 * add.cpp
 */

#include <iostream>
#include <cmath>

void add(int n, float* x, float* y) {
    for (int i = 0; i < n; ++i) {
        y[i] = x[i] + y[i];
    }
}

int main() {
    int N = 1 << 20;

    float* x = new float[N];
    float* y = new float[N];

    for (int i = 0; i < N; ++i) {
        x[i] = 1.0f;
        y[i] = 2.0f;
    }

    add(N, x, y);

    float maxError = 0.0f;
    for (int i = 0; i < N; ++i) {
        maxError = fmax(maxError, fabs(y[i] - 3.0f));
    }
    std::cout << "Max error: " << maxError << std::endl;

    delete[] x;
    delete[] y;

    return 0;
}
```

## GPU Optimization

Now, we will speed up the program by using CUDA. As a spoiler the following code runs in about **20 microseconds**, a 500x speedup! This amounts to $5\cdot 10^{10}$ floating point operations a second!

```cpp
/**
 * add.cu
 */

#include <iostream>
#include <cmath>

__global__
void add(int n, float* x, float* y) {
    int index = blockIdx.x * blockDim.x + threadIdx.x;
    int stride = blockDim.x * gridDim.x;
    
    for (int i = index; i < n; i += stride) {
        y[i] = x[i] + y[i];
    }
}

int main() {
    int N = 1 << 20;

    float* x;
    float* y;
    
    cudaMallocManaged(&x, N * sizeof(float));
    cudaMallocManaged(&y, N * sizeof(float));

    for (int i = 0; i < N; ++i) {
        x[i] = 1.0f;
        y[i] = 2.0f;
    }

    int blockSize = 256;  // number of threads in a block
    int numBlocks = 4096;  // number of blocks in the grid

    add<<<numBlocks, blockSize>>>(N, x, y);
    cudaDeviceSynchronize();

    float maxError = 0.0f;
    for (int i = 0; i < N; ++i) {
        maxError = fmax(maxError, fabs(y[i] - 3.0f));
    }
    std::cout << "Max error: " << maxError << std::endl;

    cudaFree(x);
    cudaFree(y);

    return 0;
}
```

Let's go through the changes made to the program.

The specifier `__global__` on `add` tells the CUDA C++ compiler that this is a function that runs on the GPU and can be called from CPU code (by contrast, `__device__` functions can only be called from the device). Such a function is known as a ==kernel==. Code that runs on the GPU is called ==device code==, while code that runs on the CPU is known as ==host code==.

Memory management is handled by `cudaMallocManaged()` and `cudaFree()`. These are provided by [Unified Memory](https://developer.nvidia.com/blog/unified-memory-in-cuda-6/) in CUDA, a wonderful addition which provides a single memory space accessible by all GPUs and CPUs (all the memory transfers are done automatically under the hood).

Launching CUDA kernels is done using the angle bracket syntax `<<<>>>` with two numbers: the number of requested ==blocks== in the grid and the number of ==threads== per block. Afterwards, `cudaDeviceSynchronize()` ensures that the computation on the device has finished before accessing the result.

Finally, we need to edit the kernel code to account for the fact that there are multiple threads; otherwise, each thread would just repeat the same computation. CUDA provides some keywords that allow us to determine what to run:

* `threadIdx.x` is the index of the current thread within its block.
* `blockIdx.x` is the index of the current block within the grid.
* `blockDim.x` is the total number of threads in a block.
* `gridDim.x` is the total number of blocks in the grid.

In general, these can be multi-dimensional, which is why they are accessed with `.x`. In particular, we can also pass `dim3` types into the kernel launch angle brackets `<<<>>>`. The pattern used in the kernel code is called a [grid-stride loop](https://developer.nvidia.com/blog/cuda-pro-tip-write-flexible-kernels-grid-stride-loops/).

Here's a basic PowerShell script that compiles, runs, and profiles the code. `nvcc` is the CUDA compiler, while `nsys` needs to be [installed](https://docs.nvidia.com/nsight-systems/InstallationGuide/index.html).

```PowerShell
nvcc add.cu -o add_cuda.exe
./add_cuda.exe
nsys profile --stats=true --force-overwrite true --output add_cuda_report ./add_cuda.exe
```

---

**Next:** [[Memory Hierarchy of GPUs]]