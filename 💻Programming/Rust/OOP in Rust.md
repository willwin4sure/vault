## Properties of OOP Languages

### Data and Behavior

==Objects== package data and procedures that operate on that data. In this sense, Rust is an object-oriented language, since [[Structs and Methods|structs]] hold data and `impl` blocks define behaviors on it via methods.

### Encapsulation

==Encapsulation== says that the implementation details of an object shouldn't be accessible to the code using that object. You should only interact with the public API.

We've seen how to control this using the [[Modules and Paths#Exposing Paths with `pub`|pub]] keyword. For example, you can keep the fields of a `struct` private while exposing some functions. Then, you can change the underlying representation of the object without affecting any calling code.

### Inheritance

Rust doesn't actually have inheritance. However, it has other solutions. There are two main reasons to reach for inheritance:

1. **Reuse of code.** You can do this in Rust via [[Traits#Default Behaviors|default implementation]] of traits.
2. **Polymorphism.** You can do this in Rust via trait objects, which we discuss next.

Recall that polymorphism means the ability to substitute multiple objects for each other at runtime (e.g. a function accepts a base class reference, so you can pass any derived class reference in). 

## Trait Objects

Suppose you want to make GUI library where you have a bunch of components that need to implement a `draw` method.

In a language with inheritance, you could make a `Component` base class that all the other classes inherit from. This allows you to, for example, store a vector of these `Component` s. 

In Rust, however, the method is to define a new trait with the `draw` behavior:

```rust
pub trait Draw {
	fn draw(&self);
}
```

Now, if we want to store a vector of components, we can use ==trait objects==. These are constructed using:

* Some sort of pointer, like a `&` reference or a `Box<T>` smart pointer, then
* the `dyn` keyword, then
* the trait.

For example, here is a struct containing components:

```rust
pub struct Screen {
	pub components: Vec<Box<dyn Draw>>,
}

impl Screen {
	pub fn run(&self) {
		for component in self.components.iter() {
			component.draw()
		}
	}
}
```

Here, the `dyn` keyword stands for dynamic dispatch, like how `virtual` functions work in C++. This is different than simply having a single generic type parameter with trait bounds, since you can have different concrete types stored in the same vector (as long as they all implement `Draw`).

## State Pattern

The state pattern in OOP is where objects have internal state, encoded via an object. We can do this by storing a box containing a trait object.

You could also use an [[Enums|enum]], but trait objects allow you to encapsulate transitions inside the objects themselves, rather than using a `match` in every method. This way all your object has to do is call some update functions on the state itself, without any separate logic.

---

**Next:** [[Patterns and Matching]]