## Sequential Consistency

> The result of any execution is the same as if the operations of all processors were executed in some sequential order, and the operations of each individual processor appear in this sequence in the order specified by its program.
> 
> — Leslie Lamport (1979)

Suppose two processors run the following two sequences of operations. Suppose that initially, `a = b = 0`. These operations correspond to "flag raises" for mutexes. ^bb8a14

```x86-64
mov 1, a     ; Store
mov b, %ebx  ; Load
```

```x86-64
mov 1, b     ; Store
mov a, %ebx  ; Load
```

How can we possibly analyze the behavior of this program if multiple instructions can be executed simultaneously?

> [!definition] (Sequential consistency)
> The sequences of instructions in all processors' programs are *interleaved* together to form some global *linear order* of instructions. Then, for an execution to be ==sequentially consistent==, there must exist such an interleaving such that the `LOAD` and `STORE` operations appear to obey this order, i.e. any `LOAD` receives the value of the most recent `STORE` before it.

> [!warning]
> Note that this is different than something like [[Transactions and Serializability#Serializability|serializability]]. It is a much weaker condition, we're just making sure that *parallel* executions behave as if they were executed in *some* sequential order.

Now, note that sequential consistency guarantees that there is no way for `a = b = 0` to be the result of the execution above: at least one processor will see the flag raised!

You can check this for yourself by analyzing all possible interleavings.

## Mutual Exclusion

[[Nondeterministic Parallel Programming#Implementation of Mutexes|Last]], we saw a solution to the mutual exclusion problem via a hardware atomic read-modify-write instruction `xchg`. 

> [!question] 
> Can mutual exclusion be implemented with atomic `LOAD` and `STORE` as the only memory operations?

It turns out that the answer is yes, demonstrated by Dekker and Dijkstra, at least for computers with [[Synchronization Without Locks#Sequential Consistency|sequentially consistent]] memory models. We will actually see a different algorithm for solving this problem.

> [!example] (Peterson's algorithm)
> Suppose we want to protect some variable `x` and both Alice and Bob want to operate on it. Here's the shared code:
> 
> ```c
> widget x;  // protected variable
> bool A_wants = false;
> bool B_wants = false;
> enum {A, B} turn;
> ```
> 
> Then, both Alice and Bob run the same code. Here's Alice's version:
> 
> ```c
> A_wants = true;
> turn = B;
> while (B_wants && turn == B);
> frob(&x);  // critical section
> A_wants = false;
> ```
> 
> Essentially, you raise a flag saying that you want to enter and then pass the turn to the other person. Then don't enter if its the other person's turn and they want it.

^909aa2

Alright, before we move on, let's more rigorously define the problem we are solving:

> [!definition] (Mutual exclusion with locks)
> We need to guarantee the following three properties:
> 
> * Mutual exclusion: for any threads `A` and `B` with critical section executions `lock() -> CS_{A,B} -> unlock()`, either `CS_A -> CS_B` or `CS_B -> CS_A`.
> * Deadlock freedom: if some thread calls `lock()` and never returns, then other threads must be completing `lock()` and `unlock()` calls infinitely often.
> * Starvation freedom: if some thread calls `lock()`, it will eventually return.

> [!theorem]
> Peterson's algorithm guarantees mutual exclusion, deadlock freedom, and starvation freedom.

> [!proof]- Proof of mutual exclusion
> Suppose for the sake of contradiction that Alice and Bob are both in the critical section simultaneously. Then, consider the last execution of the snippets before the critical section.
> 
> WLOG, suppose that Bob executes `turn = A;` after Alice executes `turn = B;`, so he must also execute it after Alice turns on `A_wants = true;`. But then Bob should spin, a contradiction!

> [!proof]- Proof of deadlock freedom
> If Alice and Bob both try to enter the critical section, then whoever last writes to `turn` spins and the other progresses.
> 
> If only Alice tries to enter the critical section, then she progresses since `B_wants` is false. Same for if only Bob tries to enter.

> [!question]
> Prove that Peterson's algorithm guarantees starvation freedom.

## Relaxed Consistency

> [!warning]
> Unfortunately, no modern-day processors implement [[Synchronization Without Locks#Sequential Consistency|sequential consistency]]. All implement some form of *relaxed consistency*. In particular, the hardware actively reorders instructions, and compilers may do so too.

When can this hardware reordering happen? Here's some more insight into how the memory system works:

![[store_buffer.png|center|512]]

In general, stores are buffered before sent to the memory system. Loads bypass this buffer, however, since they can easily stall the processor. However, if a load matches an address in the store buffer, they must take the value directly.

> [!example] (x86-64 total store order)
> Here is an example of a *relaxed consistency* model.
> 
> Locally:
> 
> 1. `LOAD`s are not reordered with `LOAD`s.
> 2. `STORE`s are not reordered with `STORE`s.
> 3. `STORE`s are not reordered with prior `LOAD`s.
> 4. A `LOAD` may be reordered with a prior `STORE` to a *different* location (but not one to the *same* location).
> 5. `LOAD`s and `STORE`s are not reordered with `LOCK` instructions.
> 
> Globally:
> 
> 6. `STORE`s to the same location respect a *global total order*.
> 7. `LOCK`s respect a *global total order*.
> 8. Memory ordering preserves ==transitive visibility== ("causality').

Essentially, TSO allows `LOAD` operations to flow up backwards in time to earlier locations.

Unfortunately, this means that in the [[Synchronization Without Locks#^bb8a14|initial example]] you can have an order of execution where `a = b = 0` at the end! These instruction reorders may violate sequential consistency. This also breaks [[Synchronization Without Locks#^909aa2|Peterson's algorithm]].

> [!definition] (Memory barrier)
> A ==memory fence== or ==memory barrier== is a hardware action that enforces an *ordering* constraint between the instructions before and after the fence.

This can be issued explicitly as an instruction (e.g. `mfence` in x86-64 and `atomic_strand_fence()` in C) or performed implicitly via locking, exchanging, and other synchronizing techniques. The typical cost is comparable to a L2-cache access.

> [!example] (Fenced Peterson's algorithm)
> Simply add an `atomic_thread_fence()` before the `while` spin instruction. Except the compiler might actually screw you over. You must:
> 
> * Declare variables as `volatile` to prevent the compiler from optimizing away memory references.
> * Need compiler fences around the critical sections to prevent reordering.

In general, we *will not* implement mutual exclusion using `LOAD` and `STORE` operations. This is due to the following theorem:

> [!theorem] (Burns-Lynch)
> Any $n$-thread deadlock-free mutual-exclusion algorithm using only `LOAD` and `STORE` memory operations requires $\Omega(n)$ space.

Indeed:

> [!theorem] (Attiya et al.)
> Any $n$-thread deadlock-free mutual-exclusion algorithm on a modern machine must use an expensive operation such as a *memory fence* or an *atomic compare-and-swap* operation.

## Compare and Swap Operations

In x86-64, this is supplied by the `cmpxchg` instruction. You can access it in C via `atomic_compare_exchange_strong()`. This operation atomically performs:

```c
bool CAS(T* x, T old, T new) {
	if (*x == old) {
		*x = new;
		return true;
	}

	return false;
}
```

This lets you implement mutual exclusion in $\Theta(1)$ space:

```C
void lock(int* lock_var) {
	while (!CAS(*lock_var, false, true));
}

void unlock(int* lock_var) {
	*lock_var = false;
}
```

Remember how [[Nondeterministic Parallel Programming#^4581a9|last time]] we tried to place a spin lock on the sequential summing example but ended up with a bottleneck.

Even if you prevent the bottleneck by writing to a temporary and only placing `result += temp;` in the critical section, you have an issue if the OS swaps out your thread right after you acquire the lock: this may lead to performance degradation.

> [!example] (CAS solution to summing example)
> Here is how we can leverage the compare-and-swap operation:
> 
> ```c
> int result = 0;
> cilk_for (int i = 0; i < n; ++i) {
> 	int temp = compute(myArray[i]);
> 	int old, new;
> 	do {
> 		old = result;
> 		new = old + temp;
> 	} while (!CAS(&result, old, new));
> }
> ```
> 
> This achieves synchronization without locks!

Now if the OS swaps out your thread in the critical section, others can still make progress!

> [!idea]
> This is called ==nonblocking== or ==lock-free== programming.

## Lock-Free Algorithms

These algorithms are nice since they prevent swapping-out issues and reduce the granularity of sequential code to individual instructions.

> [!example] (Lock-free stack)
> Pretty much follows the same principles as before.
> 
> ```c
> struct Node {
> 	Node* next;
> 	int data;
> };
> 
> struct Stack {
> 	Node* head;
> 	...
> }
> ```
> 
> Here are the operations:
> 
> ```C
> void push(Node* node) {
> 	do {
> 		node->next = head;
> 	} while (head != node->next || !CAS(&head, node->next, node));
> }
> 
> Node* pop() {
> 	Node* current = head;
> 	while (current) {
> 		if (head == current && CAS(&head, current, current->next)) break;
> 		current = head;
> 	}
> 
> 	return current;
> }
> ```
> 

Note that CAS acquires a cache line in exclusive mode, which invalidates the cache lines in all other caches. This results in high contention if all processors CAS to the same cache line.

This is why we use a similar trick to the spin lock, where we first read the memory location to check whether the value has changed before attempting the real atomic CAS. The shared reads lead to less cache contention. Even better, we should add [[Nondeterministic Parallel Programming#^2efefd|exponential backoff]].

> [!warning]
> In theory, threads may starve due to contention. In practice, this rarely happens.

==Transactional memory== is a field of study that may revolutionize this area, allowing blocks of code to execute atomically without worrying about locks or complicated lock-free protocols.

## ABA Problem

Here is the issue for a lock-free stack:

* Strand 1 begins to pop the head node containing `15` but is interrupted.
* Strand 2 pops the head node `15`.
* Strand 2 pops the next node `94` as well.
* Strand 2 pushes back the node with `15` but with a new value `7`.
* Strand 1 resumes execution.

Now, Strand 2 has tricked Strand 1 into thinking that the world hasn't changed, when it clearly has! You can't simply ensure that the *values* of the nodes are the same either, since maybe Strand 2 writes back the same value again.

> [!idea]
> One solution is via packing a ==version number== with each pointer.

You should increment the version every time the pointer is changed, and compare-and-swap both the pointer and version number in a single atomic operation.

---

**Return:** [[⛺Parallelization Homepage]]