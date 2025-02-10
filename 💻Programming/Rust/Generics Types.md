Think templates in C++. We've already seen several of these: `Option<T>`, `Vec<T>`, and `Result<T, E>`.

## Generic Functions

You might try to use this in the following way:

```rust
fn largest<T>(list: &[T]) -> &T {
	let mut largest = &list[0];
	for item in list {
		if item > largest {  // doesn't compile!
			largest = item;
		}
	}
	largest
}
```

You will get an error that you can't apply `>` to the type `&T`, and you should restrict to `T` with the [[Traits|trait]] `std::cmp::PartialOrd`.

## Generic Structs and Enums

You can also make generic structs. You can also use multiple ==generic type parameters==:

```rust
struct Point<T, U> {
	x: T,
	y: U,
}

impl<T, U> Point<T, U> {
	// --snip--
}
```

Of course, you can also have enums hold generic data types. You've already seen this for [[Enums#The `Option` Enum|option]] and [[Error Handling#Recoverable Errors with `Result`|result]].

You can also specify methods for certain concrete types for the generic type parameter, e.g. via

```rust
impl Point<f32, f32> { 
```

Then these methods will not exist generically.  In general, you can have even more complicated usage of generics:

```rust
impl<X1, Y1> Point<X1, Y1> {
	fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
		Point {
			x: self.x,
			y: other.y,
		}
	}
}
```

## Performance

Much like templates in C++, all generic code is turned into specific code during compilation, so it incurs no runtime cost. This process is referred to as ==monomorphization==.

---

**Next:** [[Traits]]