## Write Deterministic Programs

> [!definition] (Determinism)
> A program is ==deterministic== on a given input if every memory location is updated with the same sequence of values in every execution.

In particular, this means that the program always behaves the same way.

> [!warning]
> Two different memory locations may be updated in different orders, but each location always sees the same sequence of updates.

The biggest advantage of determinism is that it makes **debugging** simple.

> [!claim] (The golden rule of parallel programming)
> *Never* write nondeterministic parallel programs!

However, what is determinism inhibits *performance*, the goal of the class? If you must, always devise a test strategy to manage the nondeterminism, e.g.

* Turn off nondeterminism (e.g. use a debugger or locks).
* Encapsulate nondeterminism (e.g. the Cilk runtime hides nondeterminism from the programmer).
* Substitute a deterministic alternative (e.g. use a seeded PRNG).
* Use analysis tools.

> [!warning]
> You are entering the danger zone! Turn back now.

## Atomicity and Mutual Exclusion

You've seen this in [[Atomicity|systems]] and [[Transactions and Serializability|database]]. We are currently dealing with the standard threading concurrency model where the OS can choose to interrupt execution arbitrarily to context switch (unlike the [[Async Await|async/await]] model in JavaScript).

> [!definition] (Atomicity)
> A sequence of instructions is ==atomic== if the rest of the system never views them as partially executed. At any moment, it seems that either no instructions in the sequence have executed or all of them have executed.

> [!definition] (Critical section)
> A ==critical section== is a piece of code that accesses a shared data structure which must not be accessed by two or more strands at the same time. 

The primitive given to us by the OS to guarantee atomicity is the ==mutex==. Given a mutex `L`, we must lock it before entering a critical section and unlock it when we leave. This way, the operations within the critical section won't interleave.

> [!example] (Concurrent hash table)
> You can have a lock per slot to still allow some parallelization (rather than locking the whole table).

Note that such a table is *not deterministic*, since the threads can still insert elements in different orders.

Recall the definition of a [[Races and Parallelism#Determinacy Races|determinacy race]]. We can strengthen this definition to the following:

> [!definition] (Data race)
> A ==data race== occurs when two logically parallel strands *holding no locks in common* access the same memory location and at least one of the strands performs a write.

Although data race free programs often obey atomicity constraints, they can still be nondeterministic, because the act of acquiring a lock can cause a determinacy race with another lock acquisition. This means we can't use the [[Cilk#Correctness|Cilksan]] tools.

> [!warning] (No data races doesn't mean no bugs)
> You might lock the wrong thing at the wrong time, or give up a lock when you shouldn't.

> [!definition] (Benign race)
> A ==benign race== is a data race with no impact on the correctness of the program.

For example, if you are trying to identify the set of digits in an array, it doesn't matter which thread first turns a value from False to True, and overwriting it is always fine. Note this only works if the hardware writes the array elements atomically (might race on byte values for some architectures).

## Implementation of Mutexes

> [!definition] (Properties of mutexes)
> A ==yielding mutex== returns control to the OS when it blocks. A ==spinning mutex== consumes processor cycles when blocked.
> 
> A ==reentrant mutex== allows a thread that is already holding a lock to acquire it again. A ==nonreentrant mutex== deadlocks if the thread attempts to reacquire a mutex it already holds.
> 
> An ==unfair mutex== lets any blocked thread go next. One type of ==fair mutex== lets the thread that has been waiting the longest (uses a FIFO queue).

Here is an example of a spinning mutex:

```x86-64
Spin_Mutex:
	cmp 0, mutex      ; Check if mutex is free.
	je Get_Mutex
	pause             ; x86 hack to unconfuse pipeline.
	jmp Spin_Mutex

Get_Mutex:
	mov 1, %eax
	xchg mutex, %eax  ; Atomic swap to get mutex.
	cmp 0, %eax       ; Test if successful.
	jne Spin_Mutex

Critical_Section:
	<code>
	mov 0, mutex     ; Release mutex.
```

^1a8c6e

A yielding mutex has a simple change:

```x86-64
Spin_Mutex:
	cmp 0, mutex      ; Check if mutex is free.
	je Get_Mutex
	call thread_yield ; Yield the current quantum.
	jmp Spin_Mutex

Get_Mutex:
	mov 1, %eax
	xchg mutex, %eax  ; Atomic swap to get mutex.
	cmp 0, %eax       ; Test if successful.
	jne Spin_Mutex

Critical_Section:
	<code>
	mov 0, mutex     ; Release mutex.
```

The basic idea is that there is atomic swap instruction `xchg` that prevents two threads from actually grabbing the lock at the same time.

Note that there are two competing goals satisfied by these two types of mutexes:

1. To claim the mutex as soon as it is released.
2. To behave nicely and waste few cycles.

> [!idea]
> Spin for a while, and then yield. One idea is to spin for the length of a context switch.

This means you never wait more than twice the optimal time. There is a clever randomized algorithm that is $\frac{e}{e-1}$-competitive (around 1.58).

> [!example] (Summing example)
> Here's an example of some code that sums a bunch of numbers:
> 
> ```c
> int compute(const el_t *v);
> const size_t n = 1000000;
> extern el_t myArray[n];
> 
> int main() {
> 	int result = 0;
> 	cilk_for (size_t i = 0; i < n; ++i) {
> 		result += compute(&myArray[i]);
> 	}
> 	printf("The result is %d\n", result);
> 	return 0;
> }
> ```
> 
> Should we place a spin or yield lock on it?

By our existing [[DAGs, Work, and Span|work/span]] and [[Scheduling Theory and Parallel Loops|scheduling]] theory, this program has $\Theta(n)$ work and $\Theta(\log n)$ span, which means on $P$ processors the runtime is $\mathcal{O}(n/P+\log n)$. 

> [!idea]
> This is a spin mutex type of problem since you have a lot of threads doing little work, so you want to hand off the lock quickly.

By contrast, a yield mutex type of problem is one where the memory location requires a really long operation and few threads are expected to show up.

> [!example] (Summing example with spin lock)
> So you might try to solve the issue with a simple spin lock:
> 
> ```c
> int compute(const el_t *v);
> const size_t n = 1000000;
> extern el_t myArray[n];
> 
> int main() {
> 	int result = 0;
> 	pthread_spinlock_t slock;
> 	pthread_spin_init(&slock, 0);
> 	cilk_for (size_t i = 0; i < n; ++i) {
> 		pthread_spin_lock(&slock);
> 		result += compute(&myArray[i]);
> 		pthread_spin_unlock(&slock); 
> 	}
> 	
> 	printf("The result is %d\n", result); 
> 	return 0;
> }
> ```
> 
> What's the issue with this approach?

^4581a9

Now you have a sequential bottleneck so that the runtime on $P$ processors is $\Omega(n)$! 

> [!question]
> Why does the [[Nondeterministic Parallel Programming#^1a8c6e|implementation of the spin mutex]] above even have the `Spin_Mutex` part of the code? It turns out this is vital for performance.

This goes back to [[Multicore Programming#Shared-Memory Hardware|cache coherence]], the spin means that each processor simply spins on their cached value before a message in the bus to invalidate appears.

TODO: more notes on bus/cache coherence?

When the lock is freed, we do *still* have an issue where all the processors will try to `xchg` simultaneously and there will be an invalidation storm. All this traffic on the bus takes a while to quiesce, and the time can be linear in the number of processors. The reason we don't like this is because the processor that is *actually* doing work might want to access memory, and traffic on the bus may slow this down.

> [!example] (Exponential backoff spinlock)
> Replace the `pause` command in the code above with some dynamic time, which grows exponentially (e.g. doubles each time).

^2efefd

## Deadlocks

Another common topic, e.g. in [[Transactions and Serializability|database]]. These occur when you have a cycle in the wait graph of the threads. One way to solve this is to always grab locks in sorted order.

In fact, you can even deadlock with just a single lock in Cilk!

```c
void main() {
	cilk_scope {
		cilk_spawn foo();
		lock(&L);
	}
	unlock(&L);
}

void foo() {
	lock(&L);
	unlock(&L);
}
```

## Convoying

> [!definition] (Convoy)
> A lock ==convoy== occurs when multiple threads of equal priority contend repeatedly for the same lock.

For example, in Cilk's work stealing algorithm, one thread might be in the process of stealing work from another thread, and then all other threads get blocked while trying to steal as well (since they are waiting for the lock to free).

The issue here is that when a lock isn't free, you block until it is to retrieve it. The solution is to use the nonblocking function `try_lock()`, which won't block on an unsuccessful attempt to grab the lock.

---

**Next:** [[Synchronization Without Locks]]