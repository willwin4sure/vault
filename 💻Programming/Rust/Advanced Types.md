There are more use cases to the [[Advanced Traits#The Newtype Pattern|newtype pattern]]. For example, it statically enforces better typing. It can also support encapsulation by hiding some of the API.

## Type Aliases

You can also create actual ==type aliases== in Rust, e.g. via

```rust
type Kilometers = i32;
```

Now, `Kilometers` is not a separate type; you could imagine replacing any instance of it with `i32` and everything would work. However, you don't get type-checking benefits.

The main reason is to avoid repetition when you have long type names.

## The Never Type

This signifies that a function won't ever return. These are called ==diverging functions==:

```rust
fn run() -> ! {
	// --snip--
}
```

This actually explains this somewhat confusing piece of code:

```rust
let guess: u32 = match guess.trim().parse() {
	Ok(num) => num,
	Err(_) => continue,
};
```

Why would we be allowed to assign `guess` to `num` or `continue`. What's even the type of `continue`?

Well, it turns out that the answer is `!`, which can be *coerced* into any other type (since you shouldn't care anyway). Similarly, the `panic!` [[Macros|macro]] also returns `!`, so you can use it in a similar case.

## Dynamically Sized Types

Rust needs to know how much space to allocate for each value, and every type must use the same amount of memory. For example, the following code doesn't work:

```rust
// doesn't compile:
let s1: str = "Hello there!";
let s2: str = "How's it going?";
```

After all, the two strings would take up different amounts of space. The solution is to hide the `str` behind some sort of reference, e.g. via a string slice `&str` or something like a `Box<str>` or `Rc<str>`.

## The `Sized` Trait

Rust provides a `Sized` trait to determine whether a type's size is known at compile time. In fact, this gets added as a trait bound to every generic function. For example, the following code

```rust
fn generic<T>(t: T) {
    // --snip--
}
```

is treated as if it was written as

```rust
fn generic<T: Sized>(t: T) {
    // --snip--
}
```

You can relax this restriction by manually writing

```rust
fn generic<T: ?Sized>(t: &T) {
    // --snip--
}
```

which means that `T` may or may not be sized. Of course, now `t` must be of type `&T` instead of `T`, since `T` may be dynamically sized and we must therefore hide it behind some pointer.

---

**Next:** [[Advanced Functions and Closures]]