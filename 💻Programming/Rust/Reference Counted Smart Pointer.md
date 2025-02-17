Like a `shared_ptr` in C++, `Rc<T>` keeps track of its set of owners and drops when there are no more.

> [!warning]
> `Rc<T>` is only for use in single-threaded scenarios!

## Shared Data

Maybe you have some immutable cons lists and you want to share their internal representation:

```rust
enum List {
	Cons(i32, Box<List>),
	Nil,
}

use crate::List::{Cons,Nil};

fn main() {
	let a = Cons(5, Box::new(Cons(10, Box::new(Nil))));
	let b = Cons(3, Box::new(a));
	let c = Cons(4, Box::new(a));  // doesn't compile! can't take ownership
}
```

Sadly, `b` and `c` cannot both take ownership of `a`, so this won't work.

We could hold [[References and Borrowing|references]] instead, but then we would need lifetime parameters. This would then specify every element in the list lives at least as long as the entire list (otherwise you couldn't hold a reference to it inside the list). However, this is not generally true.

Here's how to do it with `Rc<T>` instead of `Box<T>`:

```rust
enum List {
    Cons(i32, Rc<List>),
    Nil,
}

use crate::List::{Cons, Nil};
use std::rc::Rc;

fn main() {
    let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
    let b = Cons(3, Rc::clone(&a));
    let c = Cons(4, Rc::clone(&a));
}
```

Every time we call `Rc::clone`, the reference count increases. (The data is not deep copied, which would be slow!). When a `Rc<T>` gets dropped, the reference count decreases. The object gets cleaned up when the ref count hits 0.

We can get the reference count by calling `Rc::strong_count` on an immutable reference. (Spoiler, there is also a `weak_count` for `Weak<T>` types... think `weak_ptr` in C++. We will discuss this soon when we talk about [[Reference Cycles|reference cycles]].)

---

**Next:** [[Interior Mutability]]
