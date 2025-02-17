This section is about how we handle errors in Rust.

> [!definition] (Types of errors in Rust)
> There are two major categories of errors ==recoverable== and ==unrecoverable== errors.
> * Recoverable errors are ones where you'd like to just report the problem to the user and retry the operation (or otherwise continue).
> * Unrecoverable errors are symptomatic of bugs and should terminate the program.

Rust doesn't actually have exceptions. It has the type `Result<T, E>` for recoverable errors and the `panic!` [[Macros|macro]] for unrecoverable errors.

## Unrecoverable Errors with `panic!`

By default, panics print an error message, unwind and clean up the stack, and then quit.

When you panic, Rust will tell you where the error occurred. You can also request a ==backtrace== by setting the environment variable `RUST_BACKTRACE=1`. This is a list of all the functions that have been called to get to this point.

> [!idea]
> The way to read a backtrace is to start from the top and read until you see files that you wrote. This is the spot where the problem originated (above and below will include core Rust code, standard library code, or external crates).

## Recoverable Errors with `Result`

A lot of errors you can handle without stopping the program. This is what the `Result` type is for:

```rust
enum Result<T, E> {
	Ok(T),
	Err(E),
}
```

Again, `T` and `E` are generic type parameters; we will discuss [[Generic Types|generics]] later. The type `T` is what is used for successes and `E` is what is used for errors.

For example:

```rust
use std::fs::File;

fn main() {
	let greeting_file_result = File::open("hello.txt");
}
```

Here, the success type is `std::fs::File` while the error type is `std::io::Error`. You can handle the variants with a match statement.

```rust
let greeting_file = match greeting_file_result {
	Ok(file) => file,
	Err(error) => panic!("Problem opening file: {error:?}"),
};
```

You can further branch on the kind of the error. For example, if the file doesn't exist, we can try to create it.

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
	let name = "hello.txt";
	let greeting_file = match File::open(name) {
		Ok(file) => file,
		Err(error) => match error.kind() {
			ErrorKind::NotFound => match File::create(name) {
				Ok(fc) => fc,
				Err(e) => panic!("Problem creating the file: {e:?}"),
			},
			other_e => {
				panic!("Problem opening the file: {other_e:?}");
			},
		},
	};
}
```

Despite the power of `match`, it is relatively primitive. [[Closures]] allow you to express the code more concisely:

```rust
let greeting_file = File::open(name).unwrap_or_else(|error| {
	if error.kind() == ErrorKind::NotFound {
		File::create(name).unwrap_or_else(|error| {
			panic!("Problem creating the file: {error:?}");
		})
	} else {
		panic!("Problem opening the file: {error:?}");
	}
});
```

There is also the `unwrap()` method on the `Result` type which calls `panic!` for us if the value is an `Err` variant, else it gives us the `T` inside of `Ok`.

Meanwhile, the `expect()` method does the same thing but also allows us to choose the error message.

## Error Propagation

When I call a function, sometimes I don't want to deal with the potential error immediately, and instead I'd like my calling code to handle it instead. This is known as ==propagating== the error, and lets us handle errors in a more centralized place.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
	let username_file_result = File::open("hello.txt");

	let mut username_file = match username_file_result {
		Ok(file) => file,
		Err(e) => return Err(e),  // propagate the error upwards
	};

	let mut username = String::new();
	match username_file.read_to_string(&mut username) {
		Ok(_) => Ok(username),
		Err(e) => Err(e),  // propagate the error upwards
	}
}
```

However, this code is a bit clunky. Rust provides the question mark operator `?` to handle this more concisely. In the `Ok` case, the `?` operator unwraps it and the program continues; in the `Err` case it propagates the error upwards by returning.

One difference from the `match` above is that in the error value case, they go through the `from` function, defined in the `From` [[Traits|trait]], which is used to convert values from one type to another. This allows type conversion from the received error type to the returned error type.

Here's a more concise version using this operator. Much of the boilerplate is removed.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
	let mut username = String::new();
	File::open("hello.txt")?.read_to_string(&mut username)?;
	Ok(username);
}
```

In fact, this function can be written simply as `fs::read_to_string("hello.txt")`.

Note that `?` can also be used on `Option` types, in which case the behavior is analogous. In the `Some` case, it unwraps it; in the `None` case, it early returns `None`.

Finally, you might want to have `main` return a `Result` type. You can do this via

```rust
fn main() -> Result<(), Box<dyn Error>> {
	let greeting_file = File::open("hello.txt")?;

	Ok(())
}
```

Here, `Box<dyn Eror>` is a [[OOP in Rust#Trait Objects|trait object]], but we don't know what that is yet. For now, it just means "any kind of error".

Now, the behavior of the executable is to return with exit code `0` on an `Ok` return, and nonzero exit code on an `Err` return.

## When to `panic!`

Panicking is unrecoverable, so you should do so with care. It is advisable to do so when your program would end up in a ==bad state==, which is when some assumption, [[contract]] (a function's precondition), or [[rep invariant]] has been violated, and at least one of the following:

* The bad state is something that is unexpected.
* Your code after this point needs to rely on not being in this bad state.
* There's not a good way to encode this information in the types you use.

However, when failure is expected, you should return a `Result` instead, e.g. running a [[parser]] on malformed data or a rate-limited HTTP request.

## Types as Validation

Using Rust's type system is one nice way of enforcing preconditions at the outset! For example, if your function only accepts values in a certain interval, you *could* make a type with a rep invariant encoding that condition. Therefore, there isn't even a way to pass in malformed data at all!

Remember to keep these types [[safe from rep exposure]].

---

**Next:** [[Generic Types]]