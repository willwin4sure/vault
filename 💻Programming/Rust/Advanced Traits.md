## Associated Types

==Associated types== specify placeholder types in trait definitions. For example, for [[Iterators|iterators]]:

```rust
pub trait Iterator {
	type Item;

	fn next(&mut self) -> Option<Self::Item>;
}
```

If you want to implement an `Iterator` for a type named `Counter` with `Item = u32`, you could write something like:

```rust
impl Iterator for Counter {
	type Item = u32;

	fn next(&mut self) -> Option<Self::Item> {
		// --snip--
}
```

The difference between this and having a generic trait like:

```rust
pub trait Iterator<T> {
	fn next(&mut self) -> Option<T>;
}
```

is that instead of being able to implement *multiple* versions of the trait for `Counter` (e.g. `Iterator<i32>` *and* `Iterator<String>`), you are only allowed a single associated type. This becomes part of the trait's contract.

## Default Generic Type Parameters

When using generic type parameters, you can specify a default concrete type. This is useful for operator overloading:

```rust
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    assert_eq!(
        Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
        Point { x: 3, y: 3 }
    );
}
```
In the trait `Add`, the default generic type is `Self`, notated as follows:

```rust
trait Add<Rhs=Self> {
    type Output;

    fn add(self, rhs: Rhs) -> Self::Output;
}
```

However, you could edit it to be something else. In general you use these in two main ways:

* To extend a type without breaking existing code.
* To allow customization in specific cases most users won't need.

## Fully Qualified Syntax for Disambiguation

Here we have a function name that is repeated in various traits:

```rust
trait Pilot {
    fn fly(&self);
}

trait Wizard {
    fn fly(&self);
}

struct Human;

impl Pilot for Human {
    fn fly(&self) {
        println!("This is your captain speaking.");
    }
}

impl Wizard for Human {
    fn fly(&self) {
        println!("Up!");
    }
}

impl Human {
    fn fly(&self) {
        println!("*waving arms furiously*");
    }
}
```

If you just call the method `fly` on some instance of a `Human`, it will call the method implemented on `Human` directly. To disambiguate, you can use `Pilot::fly(&person);` instead (pushing a reference into the `self` parameter directly).

This is called ==fully qualified syntax==. It also works with [[Structs and Methods#Associated Functions|associated functions]], but it is more complicated, since without a `self` parameter it won't know which implementation of the trait to use.

```rust
trait Animal {
    fn baby_name() -> String;
}

struct Dog;

impl Dog {
    fn baby_name() -> String {
        String::from("Spot")
    }
}

impl Animal for Dog {
    fn baby_name() -> String {
        String::from("puppy")
    }
}

fn main() {
    println!("A baby dog is called a {}", Dog::baby_name());
}
```

In this case, you can't just use `Animal::baby_name()` since `Animal` is just some random trait. If you want to call the other implementation, use `<Dog as Animal>::baby_name()`.

In general, the syntax looks like:

```rust
<Type as Trait>::function(receiver_if_method, args, ...);
```

## Supertraits

Sometimes, for a type to implement one trait, you want it to first implement some other trait. In this case, the other trait is called a ==supertrait==. 

For example, if you want some `OutlinePrint` trait to depends on `fmt::Display`, you can use the `:` syntax (like class inheritance in C++), as follows:

```rust
use std::fmt;

trait OutlinePrint: fmt::Display {
    fn outline_print(&self) {
        let output = self.to_string();
        let len = output.len();
        println!("{}", "*".repeat(len + 4));
        println!("*{}*", " ".repeat(len + 2));
        println!("* {output} *");
        println!("*{}*", " ".repeat(len + 2));
        println!("{}", "*".repeat(len + 4));
    }
}
```
## The Newtype Pattern

[[Traits#^0af148|Recall]] that you cannot implement a trait on a type unless at least one of the trait or type are local to the crate. We can get around this with the ==newtype pattern==, which just means to make a thin wrapper of an existing type with a [[Structs and Methods#Tuple Structs|tuple struct]].

Here is an example of allowing us to implement `Display` on a `Vec<String>`:

```rust
use std::fmt;

struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

fn main() {
    let w = Wrapper(vec![String::from("hello"), String::from("world")]);
    println!("w = {w}");
}
```

We have to use `self.0` to access anything (since `self` is a tuple). Further, the main downside is that `Wrapper` doesn't have any of the methods a `Vec<String>` has. One solution is implementing a [[The Deref and Drop Traits|Deref trait]] to the inner type.

---

**Next:** [[Advanced Types]]