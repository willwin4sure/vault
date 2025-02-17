Like iterators in other languages; these traverse over some sequence of items in order.

In Rust, ==iterators== are lazy, which means they have no effect until you call methods to consume them. Here is a trivial use case:

```rust
let v = vec![1, 2, 3];
for val in v.iter() {
	println!("Got: {val}");
}
```

## The Iterator Trait

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;

    // methods with default implementations elided
}
```

Here, `Item` is an ==associated type== with this trait. Basically, when you implement `Iterator`, you need to define a type `Item`, which is used in the return for `next`. `next` mutates the `Iterator` instance. Normally it returns a `Some` wrapping the next element; when it is finished, it returns `None`.

The `iter` method produces an iterator of immutable references. If you want to take ownership and return owned values, use `into_iter`. If you want mutable references, use `iter_mut`.

## Methods that Consume Iterators

==Consuming adapters== use up the iterator and produce a result, e.g. `.sum()` takes the sum of any iterator over items that implement `Sum`. It takes ownership of the iterator and uses it up.

Another common consuming adapter is `.collect()`, which packages the iterator into a collection. 

## Methods that Produce Other Iterators

==Iterator adapters== produce different iterators from the original iterator, e.g. `.map()` takes a closure and applies it to each element of the iterator.

```rust
let v1: Vec<i32> = vec![1, 2, 3];
let v2: Vec<i32> = v1.iter().map(|x| x + 1).collect();
```

Another common example is `filter`, which takes a closure that returns a `bool` specifying whether to keep the element. Remember that these closures can capture from their environment.

## Performance

Iterators are an example of a ==zero-cost abstraction==: it imposes no additional runtime overhead. This is because it gets compiled to roughly the same code. (C++ implementations tend to obey the same rule.)

In fact, using iterators can sometimes be slightly faster than writing the `for` loop manually!

---

**Next:** [[Smart Pointers]]
