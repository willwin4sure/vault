> Rust moves by default, rather than cloning. The sole owner of memory drops it when it goes out of scope (in RAII fashion). Some fundamental types are trivially copied.

This is Rust's most unique features.

> [!idea]
> Ownership enables Rust to make memory safety guarantees without needing a garbage collector.

> [!definition] 
> ==Ownership== is a set of rules that governs how Rust manages memory.

You've seen [[Storage Allocation|garbage collected and manually managed]] programming languages. Rust takes a third approach: the compiler enforces a system of ownership. None of these features slow down the program at runtime.

> [!claim] (Ownership rules)
> 1. Each value in Rust has an ==owner==.
> 2. There can only be one owner at a time.
> 3. When the owner goes out of scope, the value will be dropped.

Sounds a good amount like `unique_ptr`s in C++, or other RAII constructs for resource management.

To demonstrate the concepts of ownership, we will use the heap-allocated `String` data type.

## Rust Strings

String literals are convenient, but they are immutable and need to be known at compile-time. The `String` data type is heap-allocated, and can be mutated:

```rust
let mut s = String::from("hello");
s.push_str(", world!");
println!("{s}");
```

In Rust, instead of manually freeing memory (allocated for the string), whenever the *owner* of the memory goes out of scope, Rust calls a special `drop` function, where the author of `String` can place the code to return the memory.

## Moving Data

Here is some basic code:

```rust
let x = 5;
let y = x;
```

This binds the value `5` to both `x` and `y`. However, more complicated types behave differently.

```rust
let s1 = String::from("hello");
let s2 = s1;
```

`String` stores a data `ptr`, as well as its `len` and `capacity` on the stack (think `std::vector` in C++). Then, when we assign `s1` to `s2`, we are copying the stack data, not the heap data, so they both point to the same underlying data.

However, if both `s1` and `s2` point to the same underlying data, we will get a ==double free== error when they both fall out of scope. Rust handles this by considering `s1` as invalid after `let s2 = s1;`. We say that `s1` was ==moved== into `s2` (think `std::move` in C++).

Since only `s2` is valid, we don't face the same double free issue.

If you made `s1` mutable and assigned it to a new string, then Rust would also free the old string immediately since it falls out of scope:

```rust
let mut s = String::from("hello");
s = String::from("ahoy");
```

The original string will be `drop`ed at the second assignment.

## Cloning Data

If you *do* want to deeply copy the heap data of the `String`, we can use a common method called `clone`, e.g.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

## Copying Data

So why in our original code with integers did we not have to call `clone`, yet `x` is still valid and wasn't moved into `y`?

We are fine with this behavior because `x` is stack-allocated and copying it is inexpensive. The reason this happens is because Rust has a special annotation called the `Copy` ==trait==, which can be placed on a type to indicate that variables of that type do not move, but are rather trivially copied.

Note that a type cannot be annotated with `Copy` if the type, or any of its parts, has implemented the `Drop` trait. This is to prevent double freeing.

> [!example]
> Some types that implement `Copy` include: all [[Common Programming Concepts#Scalar Types|scalar types]] and [[Common Programming Concepts#Compound Types|tuples]] that only contain types that also implement `Copy`.

## Functions

When you pass a value into a function, it behaves similarly to assigning values to variables. It will move or copy, just like assignment.

```rust
fn main() {
	let s = String::from("hello");
	takes_ownership(s);

	let x = 5;
	makes_copy(x);
}  // here, x goes out of scope, then s. but s was moved so is invalid

fn takes_ownership(some_string: String) {
	println!("{some_string}");
}  // here, some_string goes out of scope and `drop` is called, freeing memory

fn makes_copy(some_integer: i32) {
	println!("{some_integer}");
}
```

Returning values also transfers ownership:

```rust
fn main() {
	let s1 = gives_ownership();
	let s2 = String::from("hello");
	let s3 = takes_and_gives_back(s2);
}

fn gives_ownership() -> String {
	let some_string = String::from("yours");
	some_string
}

fn takes_and_gives_back(a_string: String) -> String {
	a_string
}
```

Of course, all this ownership transfer is a bit tedious, even with tuples. Rust provides a feature for using a value without transferring ownership, which is the subject of the next section.

---

**Next:** [[Reference and Borrowing]]