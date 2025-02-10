You've seen these in other languages too. An enumeration lists a finite set of variants to choose from. However, a nice property of Rust is that each of these variants can have more data attached to them as well.

## Definition and Instantiation

An ==enumeration== is a way of saying that a value is one of some fixed possible set of values. Here is how you can define and instantiate them:

> [!example] (Basic enum usage)
> 
> ```rust
> enum IpAddrKind {
> 	V4,
> 	V6,
> }
> 
> fn main() {
> 	let four = IpAddrKind::V4;
> 	let six = IpAddrKind::V6;
> }
> ```
> 

Now suppose you wanted a type `IpAddr`, which stores both its kind *and* some associated data for the address itself. You might be tempted to use a [[Structs and Methods|struct]]:

```rust
struct IpAddr {
	kind: IpAddrKind,
	address: String,
}
```

However, there is a more concise way to represent this in Rust, where you can bundle the data directly into the enum variants:

```rust
enum IpAddr {
	V4(String),
	V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));
let loopback = IpAddr::V6(String::from("::1"));
```

Note that the enum comes implicitly with constructors that take in the arguments and construct the appropriate objects.

## Enum Values

Another advantage of using enums is that you can actually have different data for each variant, e.g.

```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}
```

The standard library actually has such a type, and they define it as follows:

```rust
struct Ipv4Addr {
	// --snip--
}

struct Ipv6Addr {
	// --snip--
}

enum IpAddr {
	V4(Ipv4Addr),
	V6(Ipv6Addr),
}
```

> [!idea]
> Enumerations allow you to handle variants in a way other than inheritance.

For example, here is a `Message` type:

```rust
enum Message {
	Quit,
	Move { x: i32, y: i32 },  // named fields like a struct
	Write(String),
	ChangeColor(i32, i32, i32),
}
```

This is similar to defining all sorts of structs (a [[Structs and Methods#Unit-Like Structs|unit struct]] for `Quit`, a normal struct for `Move`, and a [[Structs and Methods#Tuple Structs|tuple struct]] for `Write` and `ChangeColor`), but other functions can operate on `Message` types in generality.

## Enum Methods

You can also define [[Structs and Methods#Methods|methods]] on enumerations.

> [!example] (Basic method on enum)
> 
> ```rust
> impl Message {
> 	fn call(&self) {
> 		// method body would be defined here
> 	}
> }
> 
> let m = Message::Write(String::from("hello"));
> m.call();  // would invoke the method
> ```

## The `Option` Enum

Think `std::optional` in C++. Handles the case where the value could be something, or nothing. Rust doesn't have a concept of `null` like other programming languages, and for good reason.

Instead, Rust provides the following enumeration:

```rust
enum Option<T> {
	None,
	Some(T),
}
```

This is included in the Rust prelude (i.e. automatically pulled into scope). So are its variants `Some` and `None`, which you can use without the `Option::` prefix.

The `<T>` syntax is part of Rust's [[Generics Types|generics]] feature, i.e. a generic type parameter. Think templates in C++.

```rust
let some_number = Some(5);
let some_char = Some('e');

let absent_number: Option<i32> = None;
```

This is better than `null` since `Option<T>` **is a different type** than `T` itself, so you must handle it explicitly!

---

**Next:** [[Control Flow by Matching]]


