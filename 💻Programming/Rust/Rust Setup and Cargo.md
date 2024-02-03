#programming  #rust 

## Baby's First Program

[Installing Rust](https://doc.rust-lang.org/book/ch01-01-installation.html) is about as easy as installing Python.

The compiler can be invoked using `rustc <file_name>.rs`, much like `g++`. This produces an executable `<file_name>.exe` that can be run. The `<file_name>.pdb` file contains debugging information.

Here is an example program:

```rust
fn main() {
	println!("Hello, world!");
}
```

Like in C++, the `main` function runs first in any program.  `println!` is a [[Rust Macros|rust macro]] (not a function; the `!` distinguishes it) that prints to standard output with a new line. 
## Cargo

Cargo is Rust's build system and package manager (imagine CMake, but if it were as nice as pip). This is what we'll use instead of the compiler directly.

* `cargo new <project_name>` creates a new project. 

The project is also a Git repository with a `.gitignore` file. You will also see a `Cargo.toml` file, while acts like `package.json` in Typescript projects: you can specify metadata about your package, as well as its dependencies on other packages (called crates). All Rust source code should go inside `<project_name>/src/`.

* `cargo build` compiles an executable inside the `<project_name>/target/debug/` folder. Use the `--release` flag to compile with optimizations.
* `cargo run` compiles an executable and runs it immediately.

Compilation is lazy, just like with CMake. If files aren't changed, they won't be recompiled.

* `cargo check` ensures the code compiles without producing an executable.

This is faster, and a useful sanity check while programming.

## Dependencies

