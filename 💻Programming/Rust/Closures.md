Think lambdas in C++.

Now we'll start discussing some functional programming concepts. 

> [!definition] (Rust closures)
> ==Closures== are anonymous functions that capture values from the scope in which they are defined.

> [!warning]
> In Rust, inner functions do *not* capture their environment (imagine they are hoisted out), so closures are the only way to achieve this behavior.

Here is an example:

```rust
#[derive(Debug, PartialEq, Copy, Clone)]
enum ShirtColor {
    Red,
    Blue,
}

struct Inventory {
    shirts: Vec<ShirtColor>,
}

impl Inventory {
    fn giveaway(&self, user_preference: Option<ShirtColor>) -> ShirtColor {
        user_preference.unwrap_or_else(|| self.most_stocked())
    }

    fn most_stocked(&self) -> ShirtColor {
        let mut num_red = 0;
        let mut num_blue = 0;

        for color in &self.shirts {
            match color {
                ShirtColor::Red => num_red += 1,
                ShirtColor::Blue => num_blue += 1,
            }
        }
        if num_red > num_blue {
            ShirtColor::Red
        } else {
            ShirtColor::Blue
        }
    }
}

fn main() {
    let store = Inventory {
        shirts: vec![ShirtColor::Blue, ShirtColor::Red, ShirtColor::Blue],
    };

    let user_pref1 = Some(ShirtColor::Red);
    let giveaway1 = store.giveaway(user_pref1);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref1, giveaway1
    );

    let user_pref2 = None;
    let giveaway2 = store.giveaway(user_pref2);
    println!(
        "The user with preference {:?} gets {:?}",
        user_pref2, giveaway2
    );
}
```

The key is in the line `user_preference.unwrap_or_else(|| self.most_stocked())`. `unwrap_or_else` takes in an `Option<T>` and unwraps it to a `T` in the `Some` case, and calls a closure with no arguments that returns a `T` in the `None` case.

The most interesting part is that the closure is able to use `self`! This is because it *captures* an immutable reference to `self`.

## Type Inference and Annotation

Closures don't usually require you to annotate the types of the parameters and return values.

This is partly because they don't expose an interface for other users, so we don't need to specify them for documentation purposes. Further, they are often used in narrow situations where type inference is easy for the compiler.

Of course, you can add type annotations, e.g. `|x: u32| -> u32 { x + 1 }`. But you can be a lot less verbose and just write `|x| x + 1`.

## Capturing References or Moving Ownership

Closures can capture values from their environment in one of three ways:

* Borrowing immutably.
* Borrowing mutably.
* Taking ownership.

The mutable borrow will be held from the closure definition to the last time that the closure is used (so an immutable borrow won't be allowed in between, for example).

If you want the closure to forcibly take ownership, you can use the `move` keyword. This is mostly useful when passing a closure to a new thread to move the data into it.

```rust
use std::thread;

fn main() {
	let list = vec![1, 2, 3];
	println!("Before defining closure: {list:?}");

	thread::spawn(move || println!("From thread: {list:?}"))
		.join()
		.unwrap();
}
```

Generally, you want to move data into the new thread in case the current thread could die before the new thread finishes. The compiler will get mad at you if you try to remove `move`!

Further, the code in the closure may move captured values back out of the closure.

## `Fn` Traits

The way that closures interact with their environment affects which [[Traits|traits]] the closure implements; these traits are how functions and structs specify which closures they can use.

1. `FnOnce` applies to closures that can be called once. All closures implement at least this trait, because all closures can be called. A closure that moves captured values out of its body will only implement `FnOnce`, since it cannot be called more times.
2. `FnMut` applies to closures that don't move captured values out of their body, but they might mutate captured values. These closures can be called more than once.
3. `Fn` applies to closures that don't move captured values out of their body and that don't mutate the captured values. These can be called more than once without ever mutating the environment, which is important for concurrency.

For example, we can see the trait bound that `unwrap_or_else` places on its received closure:

```rust
impl<T> Option<T> {
	pub fn unwrap_or_else<F>(self, f: F) -> T
	where
		F: FnOnce() -> T
	{
		match self {
			Some(x) => x,
			None => f(),
		}
	}
}
```

The trait bound on `F` says it must be able to be called once, take no parameters, and have return type `T`. This expresses the fact that `unwrap_or_else` only cares to call `f` once. Note that you can also input the name of a function if you don't need to capture anything, e.g. `unwrap_or_else(Vec::new)`.

Here's an example of a function that implements `FnOnce` (and neither of the other two `Fn` traits).

```rust
let mut sort_operations = vec![];
let value = String::from("closure called");

let closure = |r: i32| {
	sort_operations.push(value);
	r
}
```

Here, `closure` is of type `impl FnOnce(i32) -> i32` (recall that it is standard to refer to some types just by their `impl` features, from [[Traits#^c00845|here]]). This only implements `FnOnce` since it captures `value` but then moves it out into `sort_operations`, so it can only be called once.

Therefore, you cannot use it in a method like `.sort_by_key`, which requires `FnMut` since it needs to be called multiple times, once per element in the vector.

---

**Next:** [[Iterators]]