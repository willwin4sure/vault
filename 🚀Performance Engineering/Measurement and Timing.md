> Accurate timing is key to making steady incremental progress in performance engineering.

Here is the performance engineering cycle:

1. **Measure** the performance of program `A`.
2. Make a change to `A` to produce a hopefully faster program `A'`.
3. **Measure** the performance of program `A'`.
4. If `A'` beats `A`, set `A = A'`.
5. If `A` still not fast enough, go to step 2.

## The Problem

Here is some code for timing a sorting algorithm.

```c
#include <stdio.h>
#include <time.h>

void my_sort(double* A, int n);
void fill(double* A, int n);

int main() {
	int max = 4 * 1000 * 1000;
	int min = 1;
	int step = 20 * 1000;
	
	double A[max];
	struct timespec {
		time_t tv_sec;
		long tv_nsec;
	} start, end;

	for (int n = min; n < max; n += step) {
		fill(A, n);

		clock_gettime(CLOCK_MONOTONIC, &start);
		my_sort(A, n);
		clock_gettime(CLOCK_MONOTONIC, &end);

		double tdiff = (end.tv_sec - start.tv_sec)
					 + 1e-9 * (end.tv_nsec - start.tv_nsec);

		printf("size %d, time %f\n", n, tdiff);
	}
	return 0;
}
```

You run these timings on your laptop and these are the measured running times of the sorting algorithm:

![[sorting_measured_times.png|center|512]]

Why would this happen??

> [!definition] (Dynamic voltage and frequency scaling)
> ==DVFS== is a technique to dynamically trade power for performance by adjusting the clock frequency and supply voltage to transistors. 
> 
> * Reduces operating frequency if chip is too hot or otherwise to conserve power.
> * Reduces voltage if frequency is reduced.
> * ==Turbo Boost== increases frequency if the chip is cool.

> [!claim]
> We have the identity
> $$
> P\propto CV^{2}f,
> $$
> where $P$ is the power, $C$ is the dynamic capacitance (roughly area times activity), $V$ is supply voltage, and $f$ is clock frequency.

Changing the frequency and voltage has a cubic effect on the power consumption. However, all this wreaks havoc on performance measurements!

## What to Measure

> [!question]
> You want to know which of two programs `A` and `A'` is faster, and you have a slightly noisy computer to measure their performance. 

Answer: measure them a bunch of times and perform statistics! If the [[p-values#^05c058|$p$-value]] of the null hypothesis is too low, we can reject it.

Let's say you measure a program $100$ times with the same input and interfering background noise. What statistic best represents the raw performance of the program?

> [!idea]
> Taking the **minimum** is the best for noise rejection (imagine the noise is always some additive effect on the raw performance). **Mean** is often nice for stabler results (and would be required for randomized algorithms), though it doesn't reject outliers as well as the **median**.

> [!question]
> Given several measurements, how do we report the average speedup of one program `A` over another `A'`?

We can't take the arithmetic mean of the ratios, since then the mean of `A` over `A'` values is not the reciprocal of the mean of `A'` over `A` values.

> [!idea]
> Use the **geometric mean** for ratios like speedups!

## Tools to Measure Software Performance

Sometimes you want to measure the whole program.

* e.g. `/usr/bin/time`.
* e.g. `perf stat`, `cachegrind`, `strace`.

Sometimes you want to just measure a part.

* Include timing calls in the program.
* e.g. `gettimeofday()`, `clock_gettime()`, `rdtsc()`.

Sometimes you want a profile of the program.

* e.g. `gdb`, poor man's profiler, `gprof`, `perf record/report`.
* Using sampling or instrumentation.

## Quiescing Systems

Our main goal is to reduce sources of variability in measurement. These can be caused by many things:

* Daemons and background jobs.
* Interrupts.
* Code and data alignment.
* System calls.
* OS process scheduling.
* Thread placement.
* Runtime scheduler.
* DVFS and Turbo Boost.
* Network traffic.
* Multitenancy.
* Virtualization.
* Hyperthreading (run two threads on same CPU; if one is busy with a memory fetch (say), the other can utilize the rest of the hardware). This was more relevant when processors mainly ran database code and required a lot of memory fetches. Less so now if your processors are in machine learning datacenters running models.

To ==quiesce== the system, you'll want to:

* Minimize other jobs.
	* Shut down daemons and cron jobs.
	* Disconnect the network.
	* Don't fiddle with the mouse!
	* For serial jobs, don't run on core 0, where many interrupt handlers are usually run.

* Use the Linux CPU frequency governor to control DVFS and Turbo Boost.
* Use `taskset` to pin Cilk workers to cores or hardware threads and avoid hyperthreading.

## Other Issues

A small change to source may change data alignment, which affects performance through caching/paging. 

LLVM tends to cache-align functions, but it also has several compiler switches for controlling alignment, e.g. `-align-all-functions=<uint>`, `-align-all-blocks=<uint>`, and `-align-all-nofallthru-blocks=<uint>`. Aligned code is more likely to avoid performance anomalies, but may be slower.

Even the name of the executable can affect the speed, due to alignment!

More variability:

* Memory and cache effects.
* Address-space layout randomization (ASLR).
* Off-chip communication, such as over PCI.
* Older hardware, especially hard drives.
* Different machines.
* Different compilers and libraries.
* Link order (swapping order of object files in linker command can have as much effect as `-O2` to `-O3`!)
* Interpretation and JIT compilation.
* Paths and environment variables.
* Software bugs.

---

**Next:** [[Storage Allocation]]