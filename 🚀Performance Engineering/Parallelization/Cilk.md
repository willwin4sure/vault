This is an award-winning multithreading language developed at MIT.

It provides a small set of linguistic extensions to C/C++ to support fork-join parallelism, as well as a provably efficient work-stealing scheduler. It also has a reducer linguistic interface for parallelizing code with global variables.

## OpenCilk

^c5a16a

6.106 uses the OpenCilk compiler, based on Tapir/LLVM (also developed at MIT). It can generally produce better code than other compilers for parallel language constructs. The runtime system is based on Cheetah. 

The Fibonacci code is now beautiful simple.

```c
int64_t fib(int64_t n) {
	if (n < 2) return n;
	int64_t x, y;
	cilk_scope {
		x = cilk_spawn fib(n-1);
		y = fib(n-2);
	}
	return x + y;
}
```

You cannot exit out of a `cilk_scope` until all the children function (created by `cilk_spawn`) have returned.

Note that Cilk keywords *grant permission* for parallel execution, but do not enforce it! The Cilk concurrency platform allows the programmer specify logical parallelism, which is then mapped by the scheduler onto processor cores dynamically at runtime.

## Correctness

> [!definition]
> The ==serial projection== of a Cilk program (removing all `cilk_scope` and `cilk_spawn` calls, replacing `cilk_for` with `for`) is always a legal interpretation of the program's semantics.

This allows serial testing on a single core. You can even run the parallel version of the Cilk code and force it to run on a single core.

[[Tooling#^83d6c9|Cilksan and Cilkscale]] are great tools that allow for race detection and scalability analysis.

## Reducer

TODO

---

**Return:** [[Multicore Programming#^f3db33]]
