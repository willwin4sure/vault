The Rust standard library uses a ==1:1 model== of thread implementation, whereby a program uses one OS thread per language thread. There are crates that implement other models of threading, with different tradeoffs.

## Creating a New Thread with `spawn`

We use the `thread::spawn` function and pass it a closure. For example:

```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {i} from the spawned thread!");
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("hi number {i} from the main thread!");
        thread::sleep(Duration::from_millis(1));
    }
}
```

If you run this program, you'll see that the spawned thread probably didn't finish iterating to 10! This is because when the main thread of a Rust program completes, all spawned threads are shut down.

To fix this, capture the return of `thread::spawn` in a `handle` local variable, and then call `handle.join().unwrap()`. The `join()` will cause the main thread to block until the spawned thread completes.

## Using `move` Closures with Threads

We talked about this before, in the context of [[Closures#Capturing References or Moving Ownership|closures]]. If you try to capture a variable from the main thread, the compiler will get mad at you:

```rust
use std::thread;

fn main() {
	let v = vec![1, 2, 3];

	let handle = thread::spawn(|| {
		// doesn't compile!
		println!("Here's a vector: {v:?}");
	});

	handle.join.unwrap();
}
```

The closure tries to immutably borrow `v` from the main thread, but this doesn't work since it cannot guarantee that the closure doesn't outlive `v`.

For example, if you added a `drop(v);` before the call to join the thread, and the thread happened to run after the drop, then we would be in trouble!

To fix this, we can have the closure take ownership of the vector by adding the keyword `move` before the `||`.

---

**Next:** [[Message Passing]]