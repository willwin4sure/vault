> [!definition] (Rust lifetimes)
> A ==lifetime== is the scope for which a [[References and Borrowing|reference]] is valid.

These allow Rust to guarantee the second [[References and Borrowing#^e94134|rule of references]].

## Rust's Borrow Checker

In order to prevent references from ever becoming invalid, Rust's ==borrow checker== compares scopes to determine whether all borrows are valid. For example, the following code is invalid:

```rust
fn main() {
	let r;
	{
		let x = 5;
		r = &x;
	}
	println!("r: {r}");  // fails! x does not live long enough.
}
```

Note that `r` is an uninitialized variable. This is allowed in Rust since if you try to use it then the code won't compile.

Rust rejects this program since the lifetime of `x` is shorter than the lifetime of `r`.

## Generic Lifetimes

Usually, you don't need to care much about lifetimes. Generally, the only case you care about is when you have more than one input lifetime and none of them are  `&self` or `&mut self`. 

The following code doesn't compile, since it is not clear what the lifetime of the returned reference is.

```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

We can fix this by adding ==lifetime annotations== (`'` followed by short, all lowercase name). These don't actually change the lifetimes of any of the references, but they describe their relationships.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
```

We declare the ==generic lifetime parameters== much like the [[Generics Types|generic type parameters]], inside of angle brackets. This is because we can handle arbitrary concrete lifetimes that are passed in at runtime. Note: for mutable references you'd use the syntax `&'a mut i32`. 

What is the meaning of the function signature above? Each instance of `&'a` means:

**This reference is guaranteed to live at least as long as the lifetime `'a`.**

The generic means this is true for any lifetime `'a`. Therefore, all that this signature guarantees is that the output reference lifetime is the *intersection* of the two input reference lifetimes.

> [!warning]
> Explicit lifetime annotations do not actually change the lifetimes of the values passed in or returned. Rather, they simply tell the borrow checker to reject anything that doesn't adhere to the constraints. The annotations are more for documentation and function contract specification than helping the compiler.
> 
> This is like concepts in C++. They just help the compiler point more precisely to the part of the code that is wrong. (In C++, the programmer can be told that a type doesn't obey a concept, rather than receiving a huge template error message about some random part of the code.)

In the same vein, you cannot "trick" the compiler into running bad code with incorrect lifetime parameters. For example, the following doesn't compile.

```rust
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y  // fails! returning data with lifetime 'b but needs to be 'a
    }
}
```

As another example, you can't sneak a dangling reference past Rust:

```rust
fn longest<'a>(x: &str, y: &str) -> &'a str {
	let result = String::from("really long string");
	result.as_str()  // fails! cannot return reference to local data
}
```

## Lifetime Annotations in Structs

[[Structs and Methods#^9ead01|Recall]] that we said that we can't place references to non-owned data in structs without the use of lifetimes. They require lifetime annotations on all reference members.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().unwrap();
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

The lifetime annotation encodes the fact that the struct cannot outlive the referenced data.

## Lifetime Elision

Lifetimes are technically required in tons of places. However, the compiler can automatically infer them in many situations, so you actually [don't have to care about them](https://corrode.dev/blog/lifetimes) almost all the time.

The compiler has three rules:

1. Every input reference gets a distinct input lifetime.
2. If there's exactly one input lifetime, it is applied to all output references.
3. If there are *multiple* input lifetimes but one of them is `&self` or `&mut self`, then the lifetime of `self` is applied to all output references (makes methods nicer).

If it still can't figure out the lifetime after these three rules, the compiler errors. In these cases, it requires explicit lifetime annotations.

Here's an example of the third rule:

```rust
impl<'a> ImportantExcerpt<'a> {
    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {announcement}");
        self.part
    }
}
```

## The Static Lifetime

The static lifetime denotes that the reference *can* live for the entire duration of the program. All string literals have the static lifetime:

```rust
let s: &'static str = "I have a static lifetime.";
```

This is because the text is in the program binary, which is always available. Sometimes an error message will tell you to make something `'static`, but this is usually symptomatic of a different problem.

---

**Next:** [[Automated Tests]]