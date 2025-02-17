We can pass [[Closures|closures]] into functions. You can also pass regular functions!

## Function Pointers

Functions coerce to the type `fn` (not to be confused with the [[Closures#`Fn` Traits|Fn traits]]). The `fn` type is a ==function pointer==. For example:

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
}

fn main() {
    let answer = do_twice(add_one, 5);

    println!("The answer is: {answer}");
}
```

Function pointers implement all three of the closure traits (`Fn`, `FnMut`, and `FnOnce`), since they cannot even capture things from their environment. This means it's usually better to write functions using a generic type bounded by one of the closure traits so that you can take in either closures or functions.

One time you might only want to accept `fn` and not closures is when interfacing with external code without closures, e.g. C functions can accept functions but not closures.

One use case is when using the `map` method on an iterator. You can either use a closure:

```rust
let list_of_numbers = vec![1, 2, 3];
let list_of_strings: Vec<String> =
	list_of_numbers.iter().map(|i| i.to_string()).collect();
```

or the associated function directly:

```rust
let list_of_numbers = vec![1, 2, 3];
let list_of_strings: Vec<String> =
	list_of_numbers.iter().map(ToString::to_string).collect();
```

## Returning Closures

Since closures are represented by traits, you can't return them directly since it doesn't know the size. Here is the right way to do it with a 

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
	Box::new(|x| x + 1)
}
```

Note that you could also use `impl Fn(i32) -> i32` as a return type, as we discussed for [[Traits#Traits Bounds|trait bounds]]. This would just infer the underlying concrete type, but hide it.

---

**Next:** [[Macros]] 