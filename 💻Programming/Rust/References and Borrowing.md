A ==reference== is like a pointer to access some data that is owned by some other variable. A reference is guaranteed to point to a valid value of a particular type for the life of that reference.

```rust
fn main() {
	let s1 = String::from("hello");
	let len = calculate_length(&s1);
}

fn calculate_length(s: &String) -> usize {
	s.len()
}
```

Now the function takes in a `&String` instead of a `String`, and you give it a `&s1`, which is a reference to `s1` (in C++ this would make a pointer, but it's a reference here). Note that the opposite of `&` is `*`, which is called dereferencing (same terminology in C++, but there it operates on pointers, not references).

Since inside `calculate_length`, `s` is just a reference, the string doesn't get dropped when it goes out of scope.

## Mutable References

References are immutable by default, so you cannot edit the string through a reference! We can fix this via a ==mutable reference==.

```rust
fn main() {
	let mut s = String::from("hello");
	change(&mut s);
}

fn change(some_string: &mut String) {
	some_string.push_str(", world");
}
```

> [!warning]
> Mutable references have one big restriction: if you have a mutable reference to a value, you can have no other references to that value!

The benefit of this is to prevent [[Nondeterministic Parallel Programming#^7906aa|data races]] at compile time. This happens when these three behaviors occur:

* Two or more pointers access the same data at the same time.
* At least of the pointers is being used to write to the data.
* There's no mechanism being used to synchronize access to the data.

> [!definition]
> A reference's ==scope== starts from where it is introduced and continues through the last time that the reference is used!

This means that the following code actually compiles:

```rust
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;
println!("{r1} and {r2}");
// assume r1 and r2 aren't used again.

let r3 = &mut s;  // no problem!
println!("{r3}");
```

This is because the scope of `r3` doesn't overlap with either of the scopes of `r1` or `r2`.

## Dangling References

These are a common problem in other programming languages. Let's see how Rust handles it. For example, the following code doesn't actually compile:

```rust
fn main() {
	let reference_to_nothing = dangle();
}

fn dangle() -> &String {
	let s = String::from("hello");
	&s  // reference will dangle when s goes out of scope
}
```

Of course, if you actually wanted this functionality you could just move the value out directly, removing the reference

As a reminder:

> [!idea] (Rules of references)
> 1. At any given time, you can have *either* one mutable reference *or* any number of immutable references. This is much like the [[Multicore Programming#^426837|MSI protocol]] for cache coherence.
> 2. References must always be valid.

^e94134

---

**Next:** [[The Slice Type]]