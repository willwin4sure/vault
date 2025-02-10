You've seen these in other languages. A struct bundles some data together and exposes associated methods.

## Definition and Instantiation

==Structs== are basically [[Common Programming Concepts#Compound Types|tuples]] but each element has a name, so you don't have to rely on the order to access things. The constituent pieces of data are called the ==fields== of the struct.

> [!example] (Basic struct)
> 
> ```rust
> struct User {
> 	active: bool,
> 	username: String,
> 	email: String,
> 	sign_in_count: u64,
> }
> ```

You can create ==instances== of the struct with rather intuitive syntax:

```rust
fn main() {
	let user1 = User {
		active: true,
		username: String::from("someusername123"),
		email: String::from("someone@example.com"),
		sign_in_count: 1,
	};

	println!("user1's email is: {}", user1.email);
}
```

## Shorthand Syntax

The convenient ==field init shorthand syntax==: if you ever have a variable named the same way as the field, instead of writing something like `email: email,`, you can just write `email,` directly and it will work. 

Some more syntactic sugar is the ==struct update syntax==, which allows you to copy all the fields of a struct, but change a few, e.g.

```rust
let user2 = User {
	email: String::from("another@example.com");
	..user1
};
```

Note that this moves the `username` string from `user1` into `user2`, so it is not longer valid to use the username from `user1`!

## Tuple Structs

==Tuple structs== are basically tuples with a struct name, but no names for the internal fields. For example, you can define these via:

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
	let black = Color(0, 0, 0);
	let origin = Point(0, 0, 0);
}
```

This allows you to do some more static type-checking, e.g. you cannot pass a `Point` into a function that expects a `Color`, even though the fields are the same types.

Otherwise, they behave like tuples, where you can destructure them into pieces or use `.` followed by an index to access constituent values.

## Unit-Like Structs

These structs behave like [[Common Programming Concepts#^7fd1f6|units]], where they just have a name but no data. Apparently these are useful when you need to implement a [[Traits|trait]] on some type but don't have any data that you want to store in the type itself, whatever that means.

```rust
struct AlwaysEqual;

fn main() {
	let subject = AlwaysEqual;
}
```

Eventually, we will implement behavior for this type that makes it so that every instance of `AlwaysEqual` is always equal to every instance of any other type, perhaps to have a known result for testing purposes.

> [!warning]
> So far, we've only seen structs that own all of their data. It is possible for structs to store references to data owned by something else, but to do so requires the use of [[Lifetimes|lifetimes]], which we haven't discussed yet.

^9ead01


## Derived Traits

Think dunder methods in Python that enable useful functionality. In Rust, suppose you want to print some struct, e.g.

```rust
struct Rectangle {
	width: u32,
	height: u32,
}

fn main() {
	let rect1 = Rectangle {
		width: 30,
		height: 50,
	};

	println!("rect1 is {}", rect1);  // doesn't compile!
}
```

The error says that `Rectangle` doesn't implement `std::fmt::Display`, which is what the `println!` macro uses to show primitive types.

However, it does suggest using `{:?}` instead, which uses an output format called `Debug`. In order to use this though, you need to either add the outer attribute `#[derive(Debug)]` just before the struct definition, or manually implement `Debug`. You can also pretty print with `{:#?}`.

Another way to print using the pretty `Debug` format is via the `dbg!` macro, which prints to `stderr` instead of `stdout`. It also takes ownership of an expression (unlike `println!` which takes a reference), and returns ownership of the value:

```rust
#[derive(Debug)]
struct Rectangle {
	width: u32,
	height: u32,
}

fn main() {
	let scale = 2;
	let rect1 = Rectangle {
		width: dbg!(30 * scale),
		height: 50,
	};

	dbg!(&rect1);
}
```

It also prints some useful information like the file and line numbers of the debug statements. Note that we pass a reference to `rect1` in since we don't want `dbg!` taking ownership.

## Methods

Called member functions in C++. They look like normal functions but are defined in the context of a struct (or an [[Enums|enum]] or [[trait object]]), and their first parameter is always `self`.

> [!example] (Basic `area` method for a `Rectangle`)
> 
> ```rust
> #[derive(Debug)]
> struct Rectangle {
> 	width: u32,
> 	height: u32,
> }
> 
> impl Rectangle {
> 	fn area(&self) -> u32 {
> 		self.width * self.height
> 	}
> }
> ```

Now you can invoke this method, e.g. via `rect1.area()`. Like in Python, `rect1` is automatically taken as the first argument, for `self`. About the syntax:

* `&self` is actually shorthand for `self: &Self`. Within the `impl` block, `Self` is an alias for `Rectangle` .
* Some other options include `self` which takes ownership (rare!) or `&mut self` which is a mutable borrow.

> [!warning]
> Note that Rust doesn't have the `->` operator featured in C++. This is because Rust has a feature called ==automatic referencing and dereferencing==, and calling methods is one of the few places in Rust that has this behavior.
> 
> When you call a method with `object.something()`, Rust automatically adds in `&`, `&mut`, or `*` so that `object` matches the signature of the method. This makes the language cleaner.

Like in other languages, you can have methods take in more parameters as well.

## Associated Functions

All methods are ==associated functions== to the type after `impl`. We can also define associated functions that don't have `self` as the first parameter, much like static member functions.

These don't need an instance of the type to work with, e.g. `String::from` from previous sections.

> [!example] (Basic associated function)
> 
> ```rust
> impl Rectangle {
> 	fn square(size: u32) -> Self {
> 		Self {
> 			width: size,
> 			height: size,
> 		}
> 	}
> }
> ```
>

Note that this function is namespaced by the struct, e.g. you need to access it using `Rectangle::square`.

## Multiple `impl` Blocks

You can have multiple `impl` blocks for the same struct. There usually isn't a reason to do this, but we'll see cases for this later for [[Generics Types|generic types]] and [[Traits|traits]].

---

**Next:** [[Enums]]