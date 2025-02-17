Unsafe Rust works just like regular Rust, but it doesn't enforce memory safety guarantees. This gives us extra superpowers!

This second language exists because safe Rust is inherently conservative. You can tell the compiler that you really know what you're doing. Further, computer hardware is inherently unsafe, and Rust needs to allow you to do low-level systems programming.

## Unsafe Superpowers

If you start a block with `unsafe`, you can use ==unsafe superpowers==. These include:

* Deferencing raw pointers.
* Calling unsafe functions or methods.
* Accessing or modifying a mutable static variable.
* Implementing an unsafe trait.
* Accessing fields of a `union`.

This doesn't turn off the borrow checker or disable Rust's safety checks. It just gives you access to these additional features that aren't checked by the compiler for memory safety.

To isolate unsafe code as much as possible, it is best to enclose it within a safe abstraction and offer a safe API. 

## Dereferencing a Raw Pointer

First superpower of unsafe Rust.

Unsafe Rust has two new types called ==raw pointers==, which are similar to references: these are `*const T` and `*mut T`. Raw pointers:

* Are allowed to ignore the rules of references.
* Aren't guaranteed to point to valid memory.
* Are allowed to be null.
* Don't implement any automatic cleanup.

For example, here's how you can make some raw pointers, by casting the appropriate references. This would violate the rules of reference, but we are allowed to this with raw pointers.

```rust
let mut num = 5;

let r1 = &num as *const i32;
let r2 = &mut num as *mut i32;
```
We can actually make raw pointers in safe code, but we can't dereference them unless we're in an `unsafe` block:

```rust
unsafe {
	println!("r1 is: {}", *r1);
	println!("r2 is: {}", *r2);
}
```
## Calling an Unsafe Function or Method

Second superpower of unsafe Rust.

The `unsafe` keyword on a function indicates that it has requirements we need to uphold, since Rust can't guarantee them. You have to call them in `unsafe` blocks:

```rust
unsafe fn dangerous() { }

unsafe {
	dangerous();
}
```

Bodies of `unsafe` functions are effectively `unsafe` blocks.

## Creating a Safe Abstraction Over Unsafe Code

One example of such a safe method is the method `split_at_mut`. Here's some example usage:

```rust
let mut v = vec![1, 2, 3, 4, 5, 6];

let r = &mut v[..];

let (a, b) = r.split_at_mut(3);

assert_eq!(a, &mut [1, 2, 3]);
assert_eq!(b, &mut [4, 5, 6]);
```

However, there isn't a way to implement this method with safe Rust. Here's an attempt:

```rust
fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();

    assert!(mid <= len);

	// doesn't compile!
    (&mut values[..mid], &mut values[mid..])
}
```

The issue here is that we're trying to mutably borrow from `values` twice. Rust doesn't understand that because they are nonoverlapping sections, this is actually fine.

You can turn to unsafe Rust!

```rust
use std::slice;

fn split_at_mut(values: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
    let len = values.len();
    let ptr = values.as_mut_ptr();  // makes raw pointer

    assert!(mid <= len);

    unsafe {
        (
            slice::from_raw_parts_mut(ptr, mid),
            slice::from_raw_parts_mut(ptr.add(mid), len - mid),
        )
    }
}
```

The function `slice::from_raw_parts_mut` is unsafe because it takes in a raw pointer and must trust that it is valid. The `add` method is also unsafe because it must trust that the new offset location is also a valid pointer. 

We know that this is a safe abstraction since both these operations must be valid.

## Using `extern` to Call External Code

This allows you to interact with code written in another programming language. The keyword `extern` facilitates creation and use of a ==Foreign Function Interface (FFI)==, which allows us to define functions and enable a different language to call those functions.

Here's how to integrate with the `abs` function from the C standard library. Note that functions in `extern` blocks are always unsafe to call.

```rust
extern "C" {
	fn abs(input: i32) -> i32;
}

fn main() {
	unsafe {
		println!("Absolute value of -3 according to C: {}", abs(-3));
	}
}
```

The `"C"` part defines which ==application binary interface (ABI)== the external function uses (how to call the function at assembly level).

We can also use `extern` to make an interface other programming languages can call. We also need to add the `#[no_mangle]` annotation to make sure the Rust compiler doesn't mangle the name of the function.

```rust
#[no_mangle]
pub extern "C" fn call_from_c() {
	println!("Just called a Rust function from C!");
}
```

This makes `call_from_c` accessible from C code, after it is compiled into a shared library and linked from C. Note that this usage of `extern` doesn't require `unsafe`.

## Accessing or Modifying a Mutable Static Variable

Third superpower of unsafe Rust.

Global variables are ==static variables== in Rust. These are problematic with Rust's ownership rules, since if two threads access the same mutable global variable, it can cause a data race (immutable statics are safe, however).

> [!warning]
> Immutable static variables are subtly different than [[Common Programming Concepts#Constants|constants]], since they have a fixed address in memory, so you will always access the same data. Constants are allowed to duplicate their data whenever they're used.

Static variables can also be mutable, but accessing and modifying them is unsafe. 

```rust
static mut COUNTER: u32 = 0;

fn add_to_count(inc: u32) {
    unsafe {
        COUNTER += inc;
    }
}

fn main() {
    add_to_count(3);

    unsafe {
        println!("COUNTER: {COUNTER}");
    }
}
```

Having multiple threads access `COUNTER` would likely result in a data race. Probably use a different concurrency technique.

## Implementing an Unsafe Trait

Fourth superpower of unsafe Rust.

A trait is unsafe when at least one of its methods has some invariant that the compiler can't verify.

```rust
unsafe trait Foo {
    // methods go here
}

unsafe impl Foo for i32 {
    // method implementations go here
}

fn main() {}
```

By using `unsafe impl`, we promise that we'll uphold the invariants that the compiler can't verify. For example, if we want to implement the [[Sync and Send Traits|Sync and Send traits]], we'll need to use `unsafe` to tell Rust that we're going to personally uphold that these types can be safely sent across threads or accessed from multiple threads.

## Accessing Fields of a Union

Fifth superpower of unsafe Rust.

A `union` is similar to a `struct`, but only one declared field is used at one time. These are used to interface with `union`s in C code. This is unsafe since you can't guarantee which variant is present at any time.

## When to Use Unsafe Code

Using `unsafe` for these superpowers isn't wrong or even frowned upon. But it is trickier to get `unsafe` code to be correct since the compiler can't help you out.

---

**Next:** [[Advanced Traits]]