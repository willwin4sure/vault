## Variables and Mutability

> [!idea]
> Variables are, by default, ==immutable== (like `const` variables in C++).

For example, the following code cannot compile:

```rust
fn main() {
	let x = 5;
	x = 6;  // fails! x is immutable
}
```

In order to make `x` mutable, you need to replace the first line with `let mut x = 5;`.

## Constants

You can declare a ==constant== using `const` instead of `let`. You must annotate the type. Constants must be immutable, and can be declared in any scope, including the global scope.

```rust
const THREE_HOURS_IN_SECS: u32 = 3 * 60 * 60;
```

The value that the constant gets set to must be known at compile-time; think `constexpr` in C++.

## Shadowing

You can ==shadow== a variable, *even in the same scope*. This allows you to reuse variable names rather than creating new unique ones. Therefore you can transform variables without making anything mutable.

Immutability is king, I guess, even though I feel allowing shadowing in the same scope could lead to less clear code. Note that the shadowed variable can be of different type (whereas if you use mutable variables they cannot change type).

## Data Types

Rust is ==statically typed==. Sometimes the compiler cannot infer the type without explicit help:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Without `: u32`, Rust will complain that a type annotation is needed.

> [!definition] (Scalar and compound types)
> There are two types of data types: ==scalar== types and ==compound== types. Scalars are single values, while compounds are aggregates.

There are four primary scalar types: integers, floating-point numbers, Booleans, and characters.

### Scalar Types

Here are the integer types:

| **Length** | **Signed** | **Unsigned** |
| ---------- | ---------- | ------------ |
| 8-bit      | `i8`       | `u8`         |
| 16-bit     | `i16`      | `u16`        |
| 32-bit     | `i32`      | `u32`        |
| 64-bit     | `i64`      | `u64`        |
| 128-bit    | `i128`     | `u128`       |
| arch       | `isize`    | `usize`      |

The arch lengths depend on the word size in your architecture. You can write integer literals using normal prefixes like `0x` for hexadecimal and `0b` for binary, or a type suffix like `u8`. Note that `_` can be used as a visual separator to make numbers easier to read.

For the other scalars:

* The two precisions of floating-point numbers are `f32` and `f64`.
* Booleans takes a single byte, must be `true` or `false`.
* Character literals are wrapped with single quotes.

### Compound Types

There are two primitive compound types in Rust: ==tuples== and ==arrays==. Tuples can mix types and are comma-separated, wrapped in parentheses:

```rust
fn main() {
	let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

You can also pattern match to destructure a tuple value, e.g.

```rust
let (x, y, z) = tup;
```

If you want to access individual elements of the tuple, use a `.` followed by the index, e.g. `x.0`.

> [!definition] (Unit value/type)
> The tuple without any values has a special name, ==unit==. The value and its type are written as `()`. It represents an empty value and empty return type. Expressions return this value if they don't explicitly return anything else.

^7fd1f6

Meanwhile, arrays are a collection of multiple values of the same type:

```rust
fn main() {
	let a = [1, 2, 3, 4, 5];
}
```

Arrays are *stack-allocated*! They also have fixed size. The explicit type of the array above is `[i32; 5]`. In order to initialize an array with the same value repeatedly, you can use

```rust
let a = [3; 5];
```

as an array of length `5` filled with the value `3`. Indexing is done via `a[0]`. If you index out-of-bounds, the runtime panics with a runtime error. 

## Functions

Like functions everywhere else, but you can define them anywhere as long as they're in a scope you can see:

```rust
fn main() {
	println!("Hello, world!");
	another_function(5);
}

fn another_function(x: i32) {
	println!("The value of x is {x}.");
}
```

Remember that ==parameters== are special variables that are part of the function's signatures. The concrete values that you input are called the ==arguments==. 

Let's clarify some distinctions special to Rust, an expression-based language:

> [!definition] (Statements and expressions)
> 1. ==Statements== are instructions that perform some action and do not return a value.
> 2. ==Expressions== evaluate to a resultant value.

For example, `let y = 6;` is a statement, which returns no value (unlike assignment in some other languages). Statements end with semicolons. They do things.

Pretty much everything else is an expression, like `5 + 6`. Calling a function/macro is an expression. So is any scoped block. Expressions do not end with semicolons, else you turn them into statements that don't return values.

```rust
let y = {
	let x = 3;
	x + 1
};
```

is valid Rust code; the scoped block evaluates to `4`.

> [!idea]
> The return value of a function is just the value of the final expression in the block of the body of the function.

> [!example]
> Here is a very simple function in Rust:
> 
> ```rust
> fn plus_one(x: i32) -> i32 {
> 	x + 1
> }
> 
> fn main() {
> 	let x = plus_one(5);  // has value 6
> }
> ```

## Control Flow

You can branch using `if` statements into multiple ==arms== (a term also used for `match` statements).

> [!example]
> Basic branching:
> 
> ```rust
> fn main() {
>     let number = 3;
>     if number < 5 {
>         println!("condition was true");
>     } else {
>         println!("condition was false");
>     }
> }
> ```
> 
> 

Note that the condition expression must evaluate to a `bool`, otherwise it won't compile. Things won't get coerced like in other languages.

The `if`expression also returns a value, so you can use it for assignment like a ternary operator, e.g.

```rust
let x = if condition { 5 } else { 6 };
```

There are three types of loops: `loop`, `while`, and `for`. `loop` is basically a `while true`, the others behave as you expect.

You can use `break;` statements as normal to exit a loop, but if you want to also return a value, you can add it right after, e.g.

```rust
break counter * 2;
```

When you have nested loops, Rust allows you to give the loops labels so you can break out of the outermost loop even if you are inside the inner one, e.g.

```rust
'your_label loop {
	loop {
		break 'your_label;
	}
}
```

`while` loops behave as you expect; they're `loop` statements with a condition that you check. `for` loops can be used to iterate through elements of a collection:

```rust
let a = [10, 20, 30, 40, 50];
for element in a {
	println!("the value is: {element}");
}

for number in (1..4).rev() {
	println!("{number}!");
}
println!("LIFTOFF!");
```

---

**Next:** [[Ownership]]

