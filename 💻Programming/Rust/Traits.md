> [!definition] (Rust traits)
> A ==trait== defines the functionality a particular type has and can share with other types. We can use ==trait bounds== to specify that a generic type can be any type that has certain behavior.

Traits are similar to interfaces in other languages. Trait bounds provide restrictions to generic type parameters like concepts in C++.

## Simple Example

Suppose we have `NewsArticle`s and `Tweet`s, and we want them to both be summarized. We can define a trait to encapsulate this behavior:

```rust
pub trait Summary {
	fn summarize(&self) -> String;
}
```

Each type implementing this trait must provide its own custom behavior for the body of the method. Traits can have multiple of these interface methods.

Now let's implement it!

```rust
pub struct NewsArticle {
	// --snip--
}

impl Summary for NewsArticle {
	fn summarize(&self) -> String {
		// --snip--
	}
}
```

This is similar to implementations for normal methods, but we use `impl <trait_name> for <type_name>`.

> [!warning]
> You can call trait methods like any other methods, but you must also bring the trait itself into scope!

This is why for the guessing game project you had to bring in `rand::Rng` despite not using it explicitly anywhere: this was a trait necessary to call `.gen_range` on our random number generators.

> [!warning]
> One restriction is that you can implement a trait on a type only if either the trait or the type, or both, are local to our crate. So you can:
> 
> * Implement the standard library trait `Display` on a custom type `Tweet`.
> * Implement the custom trait `Summary` on the standard library `Vec<T>`.
> 
> However, you cannot implement external traits on external types. This property is called ==coherence==, and ensures that you can't break other people's code. It also prevents ambiguity if multiple pieces of code implement the same trait for the same type.

## Default Behaviors

Sometimes it is useful to have default implementations for your traits. Think of it as an interface implementation that gets called if dynamic dispatch doesn't hit anything in the vtable.

```rust
pub trait Summary {
	fn summarize(&self) -> String {
		String::from("(Read more...)")
	}
}
```

To use this, specify an empty `impl` block, e.g.

```rust
impl Summary for NewsArticle {}
```

Default implementations can call other trait methods, even if they don't have default implementations.

## Traits Bounds

You can now specify ==trait bounds== to fix the [[Generics Types#Generic Functions|problem from last section]], e.g.

```rust
pub fn notify<T: Summary>(item: &T) {
	println!("Breaking news! {}", item.summarize());
}
```

Here, the generic type parameter `T` is constrained to implement `Summary`. Further, there is some nice syntactic sugar to rewrite this more cleanly:

```rust
pub fn notify(item: &impl Summary) {
	println!("Breaking news! {}", item.summarize());
}
```

However, the full syntax is useful if you want to, e.g. constrain two parameters to have the same type.

You can also specify multiple bounds simultaneously, e.g.

```rust
pub fn notify(item: &(impl Summary + Display)) {

// or...

pub fn notify<T: Summary + Display>(item: &T) {
```

You can also use a `where` clause for readability:

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
	T: Display + Clone,
	U: Clone + Debug,
{
```

You can also specify that a function returns some type that implements a trait, e.g.

```rust
fn returns_summarizable() -> impl Summary {
```

This is especially useful when we discuss closures and iterators, which have types that only the compiler knows or types that are very long to specify concretely. But note that your function can only return a single type when it does this (no mixing!).

## Conditionally Implemented Methods

Trait bounds let you only implement methods for generic type parameters that satisfy the trait bound.

```rust
use std::fmt::Display;

struct Pair<T> {
	x: T,
	y: T,
}

impl<T> Pair<T> {
	fn new(x: T, y: T) -> Self {
		Self { x, y }
	}
}

impl<T: Display + PartialOrd> Pair<T> {
	fn cmp_display(&self) {
		if self.x >= self.y {
			println!("x = {}", self.x);
		} else {
			println!("y = {}", self.y);
		}
	}
}
```

Here, our function requires `T` to support both comparison and printing, so we add those trait bounds.

You can also conditionally implement a method for any type that implements another trait. For example, the standard library implements `ToString` on any type that implements `Display`: it looks something like:

```rust
impl<T: Display> ToString for T {
```

After all, if you can display something to the terminal, you can always convert it to a string as well.

---

**Next:** [[Lifetimes]]