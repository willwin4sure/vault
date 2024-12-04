Developed by Intel, these are implemented as a C++ library that runs on top of native threads.

> [!idea]
> Programmers specify tasks instead of threads.

Tasks are automatically load-balanced cross threads using a ==work-stealing== algorithm inspired by research on [[Cilk]] at MIT.

Here is what Fibonacci in TBB looks like:

```cpp
#include <cstdint>
#include <iostream>
#include "tbb/task.h"

using namespace tbb;
class FibTask : public task {
public:
	const int64_t n;
	int64_t* const sum;
	
	FibTask(int64_t n_, int64_t* sum_)
		: n(n_), sum(sum_) { }

	task* execute() {
		if (n < 2) {
			*sum = n;
			
		} else {
			int64_t x, y;
			FibTask& a = *new(allocate_child()) FibTask(n-1, &x);
			FibTask& b = *new(allocate_child()) FibTask(n-2, &x);

			set_ref_count(3);
			spawn(b);
			spawn_and_wait_for_all(a);
			*sum = x + y;
		}
		
		return NULL;
	}
}

int main(int argc, char* argv[]) {
	int64_t res;
	if (argc < 2) return 1;
	int64_t n = strtoul(argv[1], NULL, 0);
	FibTask& a = *new(task::allocate_root()) FibTask(n, &res);
	task::spawn_root_and_wait(a);

	std::cout << "Fibonacci of " << n << " is " << res << std::endl;
	return 0;
}
```

Other TBB feature include:

* C++ templates to express common patterns such as:
	* `parallel_for` for loop parallelism.
	* `parallel_reduce` for data aggregation.
	* `pipeline` and `filter` for software pipelining.
	
* Thread-safe concurrent container classes. You don't want to write a concurrent [[B-tree]] every time you create a [[database]].
* Mutual-exclusion library functions including barriers, locks, and atomic updates.

---

**Next:** [[OpenMP]]