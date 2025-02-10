Rust's ==module system== consists of:

* **Packages**: A Cargo feature that lets you build, test, and share crates
* **Crates**: A tree of modules that produces a library or executable
* **Modules** and **use**: Let you control the organization, scope, and privacy of paths
* **Paths**: A way of naming an item, such as a struct, function, or module

> [!definition] (Rust crates)
> A rust ==crate== is the smallest amount of code that the Rust compiler considers at a time. There are two types of crates: ==library crates== and ==binary crates==. Library crates contain shared code for other programs, while binary crates create executables.

Most of the time, "crate" refers to library crates, e.g. the `rand` crate. These don't have a `main` function.

The ==crate root== is a source file that the Rust compiler starts from and makes up the root module of your crate.

> [!definition] (Rust packages)
> A ==package== is a bundle of one or more crates that provides a set of functionality. A package contains a `Cargo.toml` file that describes how to build them, e.g. this is what you make when you run [[Setup and Cargo#^67aac4|make a new project]] with Cargo.

A package can contain as many binary crates as you like, but at most only one library crate. A package must contain at least one crate.

Some conventions: if Rust sees a `src/main.rs` file, that is the crate root of a binary crate with the same name as the package. The analogous holds for a `src/lib.rs` file, which is the crate root of a library crate with the same name as the package.

A package can have more binary crates by simply placing files in the `src/bin` directory. Each file will be a separate binary crate.

---

**Next:** [[Modules and Paths]]