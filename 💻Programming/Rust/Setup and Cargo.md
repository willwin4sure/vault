#programming  #rust 

## Baby's First Program

[Installing Rust](https://doc.rust-lang.org/book/ch01-01-installation.html) is about as easy as installing Python.

The compiler can be invoked using `rustc <file_name>.rs`, much like `g++`. This produces an executable `<file_name>.exe` that can be run. On Windows, the `<file_name>.pdb` file contains debugging information.

> [!example] (Hello world in Rust)
> Here's a basic Rust program:
> 
> ```rust
> fn main() {
> 	println!("Hello, world!");
> }
> ```
> 
> Like in C++, the `main` function runs first in any program.  `println!` is a [[Macros|rust macro]] (not a function; the `!` distinguishes it) that prints to standard output with a new line. 

## Cargo

==Cargo== is Rust's build system and package manager (imagine CMake, but if it were as nice as pip). This is what we'll use instead of the compiler directly.

* `cargo new <project_name>` creates a new project. 

The project is also a Git repository with a `.gitignore` file. You will also see a `Cargo.toml` file, while acts like `package.json` in Typescript projects: you can specify metadata about your package, as well as its dependencies on other packages (called ==crates==). It will look something like:

```toml
[package]
name = "guessing_game"
version = "0.1.0"
edition = "2021"

[dependencies]
```

All Rust source code should go inside `<project_name>/src/`.

* `cargo build` compiles an executable inside the `<project_name>/target/debug/` folder. Use the `--release` flag to compile with optimizations.
* `cargo run` compiles an executable and runs it immediately.

Compilation is lazy, just like with CMake. If files aren't changed, they won't be recompiled.

* `cargo check` ensures the code compiles without producing an executable.

This is faster, and a useful sanity check while programming.

## Dependencies

Cargo's coordination of external dependencies is very clean. For example, in order to use the external `rand` crate, just modify your `Cargo.toml` file as follows:

```toml
[dependencies]
rand = "0.8.5"
```

This is semantic versioning, shorthand for `^0.8.5`, which means any version at least `0.8.5` but below `0.9.0`. Now building fetches the dependencies recursively and compiles them first. You can find more crates at the community registry, https://crates.io/. 

Cargo also has a mechanism to ensure reproducible builds despite potential upgrades from semantic versioning. This is via the `Cargo.lock` file (much like `package-lock.json` in TypeScript), which stores the versions that were used for the first build. Future builds just read off of `Cargo.lock`.

* `cargo update` uses your `Cargo.toml` file to update `Cargo.lock` to latest versions.

Note that since `Cargo.lock` is used for reproducibility, it should be checked into version control.

---

**Next:** [[Common Programming Concepts]]