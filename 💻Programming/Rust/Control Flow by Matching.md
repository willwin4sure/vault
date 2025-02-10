Like a `switch` statement in C++, but more powerful. Pattern matching allows safe exhaustive casework.

## Basic Matching

Patterns can be made up of literals, variables names, wildcards, and many other things. Each input is directed through the first pattern that it matches.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

## Binding Values

Now suppose that one of our variants holds some data. Then we can bind to it and use that value. Here's an example using the [[Enums#The `Option` Enum|`Option` enum]]. 

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i + 1),
	}
}
```

This pattern shows up a lot in Rust code: `match` against an enum, bind a variable to the data inside it, and then execute code based on it. Once you use it enough, you'll wish you had it in every language!

## Exhaustive Casework

> [!warning]
> One rule of Rust is that all `match` statements must be exhaustive, else the code won't compile!

For example, if you remove the `None` case above, the compiler will complain when you try to build. This will keep you safe from writing buggy code.

## Catch-all Patterns

Think `default` in C++. You have a catch-all by simply binding the value to some variable with arbitrary name. In the case that you don't want to actually use this value, you should name it `_`.

## `if let` syntax

If you only want to match a single pattern, you can use the `if let` syntax, which intuitively makes sense since `let` also performs pattern matching for binding.

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
	println!("The maximum is configured to be {max}");
}
```

If the match doesn't succeed, then the binding isn't performed and the code block isn't executed. The tradeoff for this more concise language construct is that you lose the protections of exhaustive checking.

You can also add an `else` statement which runs if the `if` block doesn't.

---

**Next:** [[Packages and Crates]]