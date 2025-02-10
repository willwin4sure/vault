How to organize code effectively in your project.

## Cheat sheet

* **Start from the crate root:** the compiler looks for the crate root, e.g. `src/main.rs` for code to compile.
* **Declaring modules:** in the crate root, you can declare new modules, e.g. via `mod garden;`. It will look for the module's code in three places:
	* Inline, within curly brackets that replace the semicolon following `mod garden`.
	* In the file `src/garden.rs`.
	* In the file `src/garden/mod.rs` (deprecated style).
* **Declaring submodules:** in any file other than the crate root, you can declare new submodules, e.g. via `mod vegetables;` inside of `src/garden.rs`. It will look for the submodule's code within the directory named for the parent module in these places:
	* Inline.
	* In the file `src/garden/vegetables.rs`.
	* In the file `src/garden/vegetables/mod.rs` (deprecated style).
* **Paths to code in modules:** once a module is part of your crate, you can refer to code in it from anywhere else in the same crate, as long as privacy rules allow, using its path, e.g. `crate::garden::vegetables::Asparagus`.
* **Private vs. Public:** code within a module is private from its parents by default. To make it public, use `pub mod` instead of `mod`. To make its items public, use `pub` before their declarations as well.
* **The `use` keyword:** you've seen this already, it creates a shortcut that replaces a long path with its final element, e.g. `use crate::garden::vegetables::Asparagus` means you can just type `Asparagus` elsewhere. Here, `crate` refers to the current crate, rather than some external one.

## Grouping Code

Modules allow code to be organized for readability and reuse. It also controls privacy, increasing abstraction since users can avoid touching internal implementation details.

## Paths

> [!definition] (Rust path)
> A ==path== is a descriptor of where to find an item in the module tree. It can take two forms:
> 
> * An ==absolute path== is the full path starting from a crate root; for code from an external crate, the absolute path begins with the crate name, and for code from the current crate, it starts with the literal `crate` (as we've seen).
> * A ==relative path== starts from the current module and uses `self`, `super` (used for the parent module), or an identifier in the current module.
> 
> Paths all consist of identifiers separated by `::`. 

In general, you should prefer absolute paths since it is more likely you will move individual pieces of code (relative paths are favored when you move multiple things at once).

## Exposing Paths with `pub`

By default, parents cannot view the internals of children, but children can access things in their parents. This encourages the hiding of implementation details, while code will always have access to the context it is defined in.

> [!warning]
> Making a module public is not sufficient for making its internals public. Those also need to be marked with `pub`.

Here's a working example:

```rust
mod front_of_house {
	pub mod hosting {
		pub fn add_to_waitlist() {}
	}
}

pub fn eat_at_restaurant() {
	crate::front_of_house::hosting::add_to_waitlist();
}
```

Note that we don't need to mark `front_of_house` as `pub` since it is a sibling to `eat_at_restaurant`, so they can refer to each other (you can see your siblings).

In addition to exposing functions with `pub`, you also can expose structs and enums. Even if you make a struct public, its individual fields will remain private: they can be revealed using `pub` on a case-by-case basis. However, making an enum public makes all its variants public.

## Creating `use` paths

Recall that `use` can create shorthand for long paths. These statements only apply to the current scope.

For idiomatic `use` paths:

* Don't bring a function into scope with `use`, bring in its parent module instead. This way you don't think that the function is local.
* Bring structs, enums, and other items fully into scope with `use`.

There is no strong reason for this idiom, but this is the convention for how people read and write Rust these days.

Obviously if you want to bring two types in and they have a name collision, you should just bring in the parent modules instead and clarify with the path.

## Providing New Names

Think `import numpy as np` in Python. You can do the same thing in Rust, using the `as` keyword to create an ==alias== for a type, e.g. 

```rust
use std::fmt::Result;
use std::io::Result as IoResult;
```

You can even re-export a name via `pub use`. This allows you organize the structure differently for programmers and users of the code.

## Nested Paths

Just some syntactic sugar to make `use` declarations take up less vertical space, e.g. via

```rust
use std::{cmp::Ordering, io};
use std::io::{self, Write};
```

## Glob Operator

You can pull all public items into scope using the `*` glob operator, e.g.

```rust
use std::collections::*;
```

---

**Next:** [[Common Collections]]