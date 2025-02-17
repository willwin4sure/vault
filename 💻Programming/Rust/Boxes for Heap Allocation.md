All this does is move the data from the stack to the heap. Here are some common situations where you want this:

1. You don't know the size of the type at compile-time but you want to use it in a place where you must know the size (you can just use a pointer instead).
2. When you want to transfer ownership of a lot of data and want to ensure no expensive copying occurs.
3. When you want to own a value, but only care about some trait it implements, not its concrete type.

Here's how you can do it:

```rust
fn main() {
	let b = Box::new(5);
	println!("b = {b}");
}
```

You can access data on the heap just as if it were on the stack. Further, when the box goes out of scope, the associated stack and heap memory both get deallocated.

Of course, you'd never just want to put an `i32` on the heap. A better example is a recursive type, like the [[cons list]] you've seen from [[⛺Software Construction Homepage|6.102]]. 

```rust
enum List {
	Cons(i32, List),  // cannot compile!
	Nil,
}
```

This can't compile, and Rust will complain that the type `List` has infinite size! Normally for an [[Enums|enum]], Rust just reserves space for the variable equal to the largest space any of the variants needs (like a `std::variant` or `union` in C++). 

The compiler suggests the following fix:

```
help: insert some indirection (e.g. a `Box`, `Rc`, or `&`) to break the cycle
```

This is because pointers have fixed size, so we won't have the same issue. Here's our new type and the usage.

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}

use crate::List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
}
```

`Box<T>` really doesn't have any extra features, but it also doesn't incur more performance overhead.

---

**Next:** [[The Deref and Drop Traits]]