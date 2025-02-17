A ==slice== allows you to reference a contiguous sequence of elements in a collection rather than the whole collection. This is a type of [[References and Borrowing|reference]], so it doesn't have ownership.

## Motivating Example

Consider a function that parses a word for the first string. One way to return this is via the index of the first space:

```rust
fn first_word(s: &String) -> usize {
	let bytes = s.as_bytes();

	for (i, &item) in bytes.iter().enumerate() {
		if item == b' ' {
			return i;
		}
	}

	s.len()
}
```

However, this function is problematic if the underlying `String` changes: what if the returned index is no longer valid due to a resize?

```rust
fn main() {
	let mut s = String::from("hello world");
	let word = first_word(&s);  // word will be 5

	s.clear();  // empties the string

	// now word is meaningless
}
```

Ideally we would like a way to make this state more connected.

## String Slices

A ==string slice== is a reference to a part of a `String`, and looks like the following:

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

Note that the ranges are exclusive at the top (both have length 5), and by placing them in brackets we construct slices that refer to portions of the string `s`. Internally, this just stores a pointer to the first element and a length.

Your range can also omit the bottom or top index, and it defaults to the extremal value. Now we can write a new function, noting that the string slice type is denoted `&str`.

```rust
fn first_word(s: &String) -> &str {
	let bytes = s.as_bytes();

	for (i, &item) in bytes.iter().enumerate() {
		if item == b' ' {
			return &s[..i];
		}
	}

	&s[..]
}
```

Now, what would happen if we tried to clear the string?

```rust
fn main() {
	let mut s = String::from("hello world");
	let word = first_word(&s);
	s.clear();  // error!
}
```

The error message will tell you that you cannot call `.clear()` since it requires a mutable reference. This is because `word` is already an immutable reference, and the [[References and Borrowing#^e94134|rules of references]] prohibit having both.

> [!idea]
> It turns out that string literals have type `&str`, i.e. they are string slices. This is also why they aren't mutable: they are just immutable references to a specific point of the binary.

An experienced Rustacean would write the parameter as `&str` type instead of `&String`, which allows both slices and strings to be passed in. You can directly pass in `String` values due to [[The Deref and Drop Traits|deref coercions]]. ^921163

## Other Slices

You can also slice arrays, e.g.

```rust
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
assert_eq!(slice, &[2, 3]);
```

These also internally store a pointer to the first element together with a length.

---

**Next:** [[Structs and Methods]]