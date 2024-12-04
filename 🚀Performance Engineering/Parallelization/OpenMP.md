Specification by an industry consortium.

Several compilers available, including open-source ones and GCC, ICC, Clang, and Visual Studio. They are linguistic extensions to C/C++ and Fortran in the form of compiler pragmas. This is what allows cross-language support!

They run on top of native threads, and support loop parallelism, task parallelism, and pipeline parallelism.

The C code is quite simple now! It also uses tasks, which can be scheduled to the cores.

```c
int64_t fib(int64_t n) {
	if (n < 2) return n;
	int64_t x, y;
#pragma omp task shared(x,n)
	x = fib(n-1);
#pragma omp task shared(y,n)
	y = fib(n-2);
#pragma omp taskwait
	return x + y;
}
```

Note that the sharing of memory is managed *explicitly* (by writing `shared(x,n)`), and then you can synchronize with a `taskwait`.

Also provides pragma directives for common patterns such as `parallel for`, `reduction`, and directives for scheduling and data sharing.

It also supplies various synchronization constructs such as barriers, atomic updates, and mutexes.

---

**Next:** [[Cilk]]