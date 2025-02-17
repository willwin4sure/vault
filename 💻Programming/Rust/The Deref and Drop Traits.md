## Dereference Operator

The `Deref` trait lets you override the behavior of the dereference operator `*`. On standard references, `*` behaves about the way you expect:

```rust
fn main() {
	let x = 5;
	let y = &x;

	assert_eq!(5, *y);
}
```

If you removed the `*`, then Rust would complain that it can't compare an `{integer}` with a `&{integer}`, as they are different types.

It turns out you can do the same thing with [[Boxes for Heap Allocation|boxes]]:

```rust
fn main() {
	let x = 5;
	let y = Box::new(x);

	assert_eq!(5, *y);
}
```

## `MyBox<T>`

We will build our own smart pointer to see what `Box<T>` is doing under the covers. The only difference is that our box will still store data on the stack, since the `Deref` trait is what we're actually interested in.

Here's the implementation:

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T> {
        MyBox(x)
    }
}

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

Now, under the hood, whenever `*y` is called, it actually runs the code `*(y.deref())`. This is why the `deref` method returns a reference; this is what the `*` operator acts upon.

## Deref Coercion

==Deref coercion== converts a reference to a type into a reference to another type. For example, this is what allows `&String` to be converted into `&str`, since `String` implements `Deref` in such a way to return a `&str`.

This is very useful in minimizing work for Rust programmers. For example, the following code compiles

```rust
fn hello(name: &str) {
    println!("Hello, {name}!");
}

fn main() {
    let m = MyBox::new(String::from("Rust"));
    hello(&m);
}
```

We are allowed to pass the type `MyBox<String>` into the `&str` parameter, by first using `deref` to case `MyBox<String>` into `&String`, and then `&String` to `&str`.

Without any coercion, the line would look like `hello(&(*m)[..])`... what a mouthful!

## Mutable Deref

So far, we've seen how `Deref` lets us override `*` on immutable references. For mutable references, we just need to use the `DerefMut` trait instead. Here are the rules:

1. `&T` goes to `&U` when `T: Deref<Target = U>`.
2. `&mut T` goes to `&mut U` when `T: DerefMut<Target = U>`
3. `&mut T` goes to `&U` when `T: Deref<Target = U>`

Going from mutable to immutable can never violate borrowing rules. But you can't go the other way.

## The `Drop` Trait

Allows you to override what happens when Rust calls `drop` on an object going out of scope. For example:

```rust
struct CustomSmartPointer {
    data: String,
}

impl Drop for CustomSmartPointer {  // trait included in Rust prelude
    fn drop(&mut self) {
        println!("Dropping CustomSmartPointer with data `{}`!", self.data);
    }
}

fn main() {
    let c = CustomSmartPointer {
        data: String::from("my stuff"),
    };
    let d = CustomSmartPointer {
        data: String::from("other stuff"),
    };
    println!("CustomSmartPointers created.");
}
```

Note that variables are dropped in reverse creation order, like a stack.

## Dropping a Value Early

Sometimes you want to do this, e.g. with a smart pointer that manages locks. To avoid double free errors, you have to use the `std::mem::drop` function (also included in the prelude).

---

**Next:** [[Reference Counted Smart Pointer]]