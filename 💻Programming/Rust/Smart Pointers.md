Originated from smart pointers in C++. While [[References and Borrowing|references]] are the simplest type of pointer in Rust, ==smart pointers== have additional metadata and capabilities.

In fact, you've already seen a couple smart pointers: `String` and `Vec<T>` both own some memory (and associated metadata) and allow you to manipulate it.

Smart pointers are usually implemented via [[Structs and Methods|structs]], though they also implement the `Deref` and `Drop` traits. The `Deref` trait allows an instance of a smart pointer to be used like a reference, so your code can work with either. The `Drop` trait allows you to customize the code that is run when the smart pointer falls out of scope (like a destructor).

We will talk about a few common smart pointers:

* Want something on the heap? Put it in a `Box<T>`.
* `Rc<T>` uses reference counting to allow multiple ownership (think `shared_ptr` in C++).
* `Ref<T>` and `RefMut<T>` , accessed through `RefCell<T>`, a type that enforces borrowing rules at runtime instead of compile time.

We'll also talk about ==interior mutability==: an immutable type exposes an API to mutate interior state (you saw this in [[⛺Software Construction Homepage|6.102]]), and ==reference cycles== (you saw this in [[Storage Allocation#Reference Counting|6.106]]). 

1. [[Boxes for Heap Allocation]]
2. [[The Deref and Drop Traits]]
3. [[Reference Counted Smart Pointer]]
4. [[Interior Mutability]]
5. [[Reference Cycles]] ^3399f9

---

**Next:** [[Fearless Concurrency]]
