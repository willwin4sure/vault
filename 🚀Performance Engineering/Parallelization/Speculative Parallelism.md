> You guess that it's worthwhile to compute something, but you might be wrong.

If you're right, you get to do work in parallel and it runs faster. If you're wrong, you end up wasting resources.

Here's a basic program you might want to speculatively parallelize:

```c
bool find(int* S, int n, int key) {
	assert(n > 0);

	for (int i = 0; i < n; ++i) {
		if (S[i] == key) return true;
	}

	return false;
}
```

Here's how we can short-circuit a D&C version:

```c
void p_find_helper(int* S, int n, int key, bool* found) {
	if (*found) return;
	if (n == 1) {
		if (S[n] == key) *found = true;
	} else cilk_scope {
		cilk_spawn p_find_helper(S, n/2, key, found);
		if (*found) return;
		p_find_helper(S + n/2, n - n/2, key, found);
	}
	return;
}

bool p_find(int* S, int n, int key) {
	bool found = false;
	p_find_helper(S, n, key, &found);
	return found;
}
```

Notes:

* This is [[Nondeterministic Parallel Programming|nondeterministic programming]]! We don't like that.
* The benign race on `&found` is dangerous and technically undefined behavior in C. Should instead use `int* found` for it to be atomic.

> [!definition] (Speculative parallelism)
> ==Speculative parallelism== occurs when a program spawns a parallel task that would not be performed by the serial projection.

A good rule of thumb is to not attempt speculative parallelism unless there are no opportunities for parallelization elsewhere.

