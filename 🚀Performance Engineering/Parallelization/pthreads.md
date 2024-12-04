`pthreads` is the standard API for threading specified by ANSI/IEEE POSIX 1003.1-2008. It is a "do-it-yourself" concurrency platform.

It is built as a library of functions with "special" non-C semantics. Each thread implements its own abstraction of processor, which are multiplexed onto machine resources.

Here are the APIs for the key pthread functions that create and join threads.

```c
int  // returns error status
pthread_create(
	pthread_t* thread           // returned identifier for new thread
	const pthread_attr_t* attr  // object to set thread attributes (or NULL)
	void* (*func)(void*)        // routine executed after creation
	void* arg                   // a single argument passed to func
)
```

```c
int  // returns error status
pthread_join(
	pthread_t thread  // identifier for thread to wait for
	void** status     // terminating thread's status
)
```

The new code using these primitives is rather involved.

```c
int main(int argc, char* argv[]) {
	pthread_t thread;
	thread_args args;  // Struct allowing in/out parameters.
	int status;
	int64_t result;

	if (argc < 2) return 1;
	int64_t n = strtoul(argv[1], NULL, 0);
	if (n < 30) {
		result = fib(n);
		
	} else {
		args.input = n - 1;
		
		// Create other thread to handle one branch.
		status = pthread_create(&thread, NULL, thread_func, (void*)&args)
		if (status != NULL) return 1;

		// Compute the other branch yourself.
		result = fib(n-2);

		// Wait for thread to finish.
		status = pthread_join(thread, NULL);
		if (status != NULL) return 1;

		result += args.output;
	}

	printf("Fibonacci of %" PRId64 " is %" PRId64 ".\n", n, result);
	return 0;
}
```

Some issues:

* Creating a thread takes $>10^{4}$ cycles of overhead. Thread pools can help.
* Fibonacci code gets around 1.5 speedup with 2 cores, since the two branches have different sizes (only get golden ratio). Also need a rewrite for more cores.
* Fibonacci code is no longer neatly encapsulated in `fib()`.
* Programmers much marshal arguments and engage in error-prone protocols to load-balance.

---

**Next:** [[Threading Building Blocks]]