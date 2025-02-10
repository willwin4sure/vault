We'll look at three types of collections: vectors, strings, and hash maps.

## Vectors

You can create empty vectors, or use the macro `vec!`:

```rust
let v: Vec<i32> = Vec::new();
let v = vec![1, 2, 3];
```

You can append to a vector with `push`:

```rust
let mut v = Vec::new();
v.push(5);
```

There are two ways to obtain references to values stored in a vector. One is by direct indexing, which panics if the index is out of bounds. The other way is to `get` an [[Enums#The `Option` Enum|optional value]]:

```rust
let v = vec![1, 2, 3, 4, 5];
let third: &i32 = &v[2];
let third: Option<&i32> = v.get(2);
```

You can then handle the `Option` value with [[Control Flow by Matching|matching]]. Note that the same [[References and Borrowing#^e94134|rules of references]] apply, and the following code doesn't compile:

```rust
let mut v = vec![1, 2, 3];
let first = &v[0];
v.push(6);
println!("The first element is: {first}");  // fails!
```

This is because the scope of the immutable borrow `first` intersects the attempt at a mutable borrow when calling `push`.

We like this behavior, since mutating `v` via `push` may cause the vector to resize, leading to reallocation on the heap.

Here's how you can iterate over values in a vector:

```rust
let v = vec![100, 32, 57];
for i in &v {
	println!("{i}");
}
```

Note that, somewhat confusingly, the iterator through `&v` also returns references to the elements inside of `v`. 

If you wanted to modify the elements of `v`, you could do so with a mutable borrow:

```rust
let v = vec![100, 32, 57];
for i in &mut v {
	*i += 50;
}
```

Iteration is safe due to the borrow checker's rules: you cannot insert or remove items in the `for` loop bodies since the reference to `v` prevents simultaneous modification.

Note that one way of grouping objects of different types into a vector is via an enum. In other languages, you might have to resort to inheritance, but this is a nice property of Rust.

Finally, note that dropping a vector also drops its elements.

## Strings

A `String` is essentially a wrapper around a vector of bytes with some extra guarantees, restrictions, and capabilities.

We can create some string using the `to_string` method, which exists on any type that implements the `Display` [[Traits|trait]], e.g. string literals.

```rust
let s = "initial contents".to_string();
```

Of course, you can also use the associated function `String::from` as we've seen before. Strings are UTF-8 encoded, you so you can place all sorts of data in them, including e.g. Chinese characters.

You can push more data into a string.

```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);  // doesn't take ownership of s2
println!("s2 is {s2}");
```

You can also use the `+` operator.

```rust
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2;  // note s1 is moved out and is invalid
```

We can see what's going on under the hood by looking at the signature of the `add` method (used by the `+` operator):

```rust
fn add(self, s: &str) -> String {
```

So what's actually happening is that `s3` takes over the memory of `s1` and then appends the contents of `s2` to the end of it.

One subtle thing is that `s2`, as a `&String`, is being [[deref coerced]] into a `&str` type when passed as an argument to this method.

Finally, another option is the `format!` macro, which basically behaves like `println!` but returns a `String` instead of outputting to terminal.

```rust
let s = format!("{s1}-{s2}");
```

> [!warning]
> In Rust, strings do not support indexing.

This is due to how strings are stored in memory as UTF-8 encoded data, as some characters require multiple bytes to store. So indexing would be potentially confusing for the programmer and/or inefficient to run, at best.

If you slice strings with `[]`, you will be slicing the bytes and therefore might cut characters in half (causing a panic!). So the actual best way to operate on pieces of strings is by deciding whether you want characters of bytes, via `.chars()` or `.bytes()`.

## Hash Maps

Stores key-value pairs via a hashing function on the keys.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("blue"), 10);
scores.insert(String::from("yellow"), 50);

let team_name = String::from("blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

The `.copied()` converts the returned `Option<&i32>` into an `Option<i32>` by copying in the value. Then, `unwrap_or` handles the optional by returning `0` in the `None` case.

You can also iterate (in arbitrary order):

```rust
for (key, value) in &scores {
	println!("{key}: {value}");
}
```

When you call `insert`, the map will take ownership of the values (unless it implements the `Copy` [[Traits|trait]]). You could also push in references instead, but the values that they point to must be valid for at least as long as the hash map is valid.

Note that calling `insert` multiple times with the same key will result in overriding. If you only want to add a key-value pair if that key isn't already present:

```rust
scores.entry(String::from("yellow")).or_insert(50);
```

This grabs the (potentially empty) entry from the map, and then inserts the new value if it doesn't exist already.

You can also use this to update the entry, since it returns a view into it. For example, if you want a frequency map, do the following:

```rust
for word in text.split_whitespace() {
	let count = map.entry(word).or_insert(0);
	*count += 1;
}
```

By default, `HashMap` uses the *SipHash* function which has some security features. You can switch to a different ==hasher== if you want better performance; this is any type that implements the `BuildHasher` [[Traits|trait]].

---

**Next:** [[Error Handling]]