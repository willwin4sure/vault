If you make a reference cycle with `Rc<T>` or `RefCell<T>`, it will memory leak. Rust doesn't guarantee prevention of memory leaks.

## `Weak<T>` to Avoid Cycles

Think `weak_ptr` in C++.

We've seen that `Rc::clone()` on a reference to a `Rc<T>` increases the `strong_count` by 1 and gives you a new `Rc<T>`. You can also create a ==weak reference== to the value inside a `Rc<T>` by calling `Rc::downgrade()` to increase the `weak_count` by 1 and receiving a new `Weak<T>`.

Since `Weak<T>` may reference values that have been dropped, you must always check if the reference is still valid. You do this with the `upgrade` method on a `Weak<T>`, which promotes it to an `Option<Rc<T>>` (depends on if it succeeds).

## Tree Data Structure

```rust
use std::cell::RefCell;
use std::rc::Rc;

#[derive(Debug)]
struct Node {
	value: i32,
	children: RefCell<Vec<Rc<Node>>>;
}
```

Each node owns its own children. We also want to be able to share that ownership with (local) variables to access each node directly, so the underlying type is `Rc<Node>` (we have shared pointers). We also want to modify which nodes are children of other nodes, so we put the entire `Vec` inside of a `RefCell`.

So you can make a parent and child like this:

```rust
fn main() {
	let leaf = Rc::new(Node {
		value: 3,
		children: RefCell::new(vec![]),
	});

	let branch = Rc::new(Node {
		value: 5,
		children: RefCell::new(vec![Rc::clone(&leaf)]),
	});
}
```

Note that the child node is owned both by `leaf` and by `branch` via its `children`. Now, suppose we want to add back-pointers up the tree. One clean way to do this is by using `Weak<T>`.

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

#[derive(Debug)]
struct Node {
	value: i32,
	parent: RefCell<Weak<Node>>,
	children: RefCell<Vec<Rc<Node>>>,
}

fn main() {
	let leaf = Rc::new(Node {
		value: 3,
		parent: RefCell::new(Weak::new()),
		children: RefCell::new(vec![]);
	});

	let branch = Rc::new(Node {
		value: 5,
		parent: RefCell::new(Weak::new()),
		children: RefCell::new(vec![Rc::clone(&leaf)]),
	});

	*leaf.parent.borrow_mut() = Rf::downgrade(&branch);
}
```

This constructs the same two nodes but sets the parent of the leaf to be the branch. You can check the value of the parent via `leaf.parent.borrow().upgrade()`.

---

**Return:** [[Smart Pointers#^3399f9]]