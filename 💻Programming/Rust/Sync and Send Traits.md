The Rust language itself doesn't have many concurrency features. The things we've seen so far are just part of the standard library.

In this section we talk about two concurrency concepts embedded in the language itself: the `std::marker` traits `Sync` and `Send`.

## The `Send` Marker

Types implementing `Send` can have ownership transferred between threads. For example, `Rc<T>` doesn't implement `Send`, while `Arc<T>` does, which protects you from making a mistake.

Any type composed entirely of `Send` types is automatically marked as `Send` as well. Almost all primitive types are `Send`, aside from raw pointers.

## The `Sync` Marker

Types implementing `Sync` are safe to be referenced from multiple threads. In other words, any type `T` is `Sync` if `&T` is `Send`. For example, `RefCell<T>` does not implement `Sync` (since its runtime borrow checking is not thread-safe), while `Mutex<T>` is `Sync`.

Primitive types are `Sync`, and types composed entirely of types that are `Sync` are also `Sync`.

## Manual Implementation is Unsafe

What the header says; manually implementing these traits requires unsafe Rust code.

---

**Return:** [[Fearless Concurrency#^d5b601]]
