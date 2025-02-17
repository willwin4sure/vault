With a shared state, multiple threads can access the same memory location. You already know the dangers of this: you have to deal with locking, and where there's locking, you have to deal with deadlock.

## Mutual Exclusion with `Mutex<T>`

This is the multi-threaded version of [[Interior Mutability#Runtime Borrow Checking with `RefCell<T>`|reference cells]], i.e. `RefCell<T>` (it also provides [[Interior Mutability|interior mutability]]). A mutex ==guards== the data it holds via the locking system, and only allows a single thread to access it at a time.

You have to acquire the lock before using the data, and unlock it when you are done. In Rust, the `Mutex` itself holds the data:

```rust
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);

    {
        let mut num = m.lock().unwrap();
        *num = 6;
    }

    println!("m = {m:?}");
}
```

Note that the `lock` method blocks the current thread until it can acquire the lock. It would fail if any other thread holding the lock panicked. We `unwrap` to panic ourselves, in this case.

The `lock` method returns a `MutexGuard` wrapped in a `LockResult`. `MutexGuard` is a [[Smart Pointers|smart pointer]] that implements `DerefMut` to point at the inner data. It also implements `Drop` to release the lock at end of scope.

However, if we try to share a `Mutex` across threads naively, we will run into ownership issues, since we can't move the object into multiple threads. We can try multiple ownership with `Rc<T>`, but that is not thread-safe.

We need another solution.

## Atomic Reference Counting with `Arc<T>`

This is the multi-threaded version of [[Reference Counted Smart Pointer|reference counted smart pointers]], i.e. `Rc<T>`. It stands for an ==atomically reference counted== type.

Just as you could combine `RefCell<T>` and `Rc<T>` into `Rc<RefCell<T>>` so that you can share mutable state, you can do the same thing with `Arc<Mutex<T>>`. Here's an example of having multiple threads share a counter:

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            let mut num = counter.lock().unwrap();

            *num += 1;
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", *counter.lock().unwrap());
}
```

You can also see the `std::sync::atomic` module for other thread-safe primitives.

## Deadlocks

Note that `Mutex<T>` still comes with the risk of creating ==deadlocks==. Rust can't protect you from this type of logic error.

---

**Next:** [[Sync and Send Traits]]