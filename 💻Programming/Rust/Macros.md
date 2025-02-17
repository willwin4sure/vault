Finally, we will discuss what these are.

There are two types of ==macros== in Rust. First, there are ==declarative== macros with `macro_rules!`. There are also three kinds of ==procedural== macros:

* Custom `#[derive]` macros that specify code added with the `derive` attribute used on structs and enums.
* Attribute-like macros that define custom attributes usable on any item.
* Function-like macros that look like function calls but operate on the tokens specified as their argument.

## Macros v.s. Functions

Macros fall under ==metaprogramming==; they expand to produce more code than you've written manually.

In general, macros have some more powers than function calls, since they are expanded before the compiler interprets the meaning of the code. For example, this means that a macro can implement a trait on a given type, unlike a function.

However, macro definitions are more complex than function definitions. Further, you must bring them into scope *before* you call them, since the compiler needs to know about them immediately.

## Declarative Macros

These sallow you to write something similar to a Rust `match` expression. For example, `vec!` can be used to make a vector of all different types of sizes and types. Here's a simplified definition:

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

The actual standard library code will pre-allocate the size of the vector as an optimization. Let's break down the simplified code:

* The `#[macro_export]` annotation indicates that it should be made available whenever the crate in which the macro is defined is brought into scope.
* Write `macro_rules!`, followed by the name of the macro *without* the exclamation mark.
* The code inside is similar to a `match`. We only have one arm: when the code inside matches the pattern `( $( $x: expr ),* )`, it emits the block of code afterwards. If the code doesn't match any arm, it errors.
* We use `$` to declare a variable in the macro system. For example, `$x:expr` matches any Rust expression and gives it the name `$x`.
* The `,` means that the literal comma separator character could optionally appear after the code that matches in `$()`. The `*` specifies that the pattern matches zero or more of whatever precedes the `*` (like in regex).

## Procedural Macros

These act more like a function. They accept code as an input, operate on that code, and produce some code as an output.

When creating procedural macros, the definitions must reside in their own crate with a special crate type (due to annoying complex technical reasons).

```rust
use proc_macro;

#[some_attribute]
pub fn some_name(input: TokenStream) -> TokenStream {
}
```

The procedural macro must take in some `TokenStream` (defined in the crate `proc_macro`) and produce some output `TokenStream`. The attribute attached specifies which kind of procedural macro we are creating.

### Custom `derive` Macros

We want users to be able to write code like this:

```rust
use hello_macro::HelloMacro;
use hello_macro_derive::HelloMacro;

#[derive(HelloMacro)]
struct Pancakes;

fn main() {
    Pancakes::hello_macro();
}
```

To do this, we make a new library crate called `hello_macro`. We define the trait.

```rust
pub trait HelloMacro {
    fn hello_macro();
}
```

The user could now implement the trait for the desired functionality:

```rust
use hello_macro::HelloMacro;

struct Pancakes;

impl HelloMacro for Pancakes {
    fn hello_macro() {
        println!("Hello, Macro! My name is Pancakes!");
    }
}

fn main() {
    Pancakes::hello_macro();
}
```

However, we can't specify a default implementation. In particular, we can't look up the type name at runtime, so we need to do it with a macro at compile time.

We do this by making a new crate called `hello_macro_derive` inside of our `hello_macro` project. We place these two crates in the same project since they are tightly coupled. We publish them as independent crates so you can use `hello_macro` without necessarily using the `derive` functionality.

To define it as a procedural macro crate, you should place the following in `Cargo.toml` for the `hello_macro_derive` crate:

```rust
[lib]
proc-macro = true

[dependencies]
syn = "2.0"
quote = "1.0"
```

Here's how you can start coding in the `lib.rs` file:

```rust
use proc_macro::TokenStream;
use quote::quote;

#[proc_macro_derive(HelloMacro)]
pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
    // Construct a representation of Rust code as a syntax tree
    // that we can manipulate
    let ast = syn::parse(input).unwrap();

    // Build the trait implementation
    impl_hello_macro(&ast)
}
```

The outer function parses the abstract syntax tree and hands it off to the inner function for the actual implementation.

By specifying the annotation `#proc_macro_derive(HelloMacro)`, whenever the user uses `#[derive(HellowMacro)]` on a type, `hello_macro_derive` will be called.

Here's an actual implementation:

```rust
fn impl_hello_macro(ast: &syn::DeriveInput) -> TokenStream {
    let name = &ast.ident;
    let gen = quote! {
        impl HelloMacro for #name {
            fn hello_macro() {
                println!("Hello, Macro! My name is {}!", stringify!(#name));
            }
        }
    };
    gen.into()
}
```

The `quote!` macro lets us write the Rust code we want to return. The `into` method conveniently converts it into a `TokenStream`. It also has nice templating mechanics: `#name` will be replaced with the value in the variable `name`.

Note that the `stringify!` macro is built into Rust, and converts expressions into string literals at compile time.

As a user of the crate, you can now pull the new crates from the registry if they've been uploaded. Otherwise, you can use `path` dependencies:

```toml
hello_macro = { path = "../hello_macro" }
hello_macro_derive = { path = "../hello_macro/hello_macro_derive" }
```

### Attribute-like Macros

These let you create new attributes too, and are more flexible (can apply to functions, note just structs and enums). For example, a user may call:

```rust
#[route(GET, "/")]
fn index() {
```

Now, the signature of the macro definition looks like:

```rust
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
```

The first parameter is the contents of the attribute, i.e. `GET, "/"`. The second parameter is the body that the attribute is attached to, i.e. `fn(index) {` and the rest of the function body.

Then, you just generate the token stream that you want!

### Function-like Macros

These look like function calls, but are much more flexible. Unlike `macro_rules!` that uses match-like syntax, these just take in a `TokenStream` and produce a new one. For example, you might have a user call:

```rust
let sql = sql!(SELECT * FROM posts WHERE id=1);
```

Then, the macro could do extra things like checking that the SQL statement is valid. The signature of the macro definition looks like:

```rust
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
```

We receive the tokens from inside the parentheses, and then return the code we want to generate.

---

**Return:** [[⛺Rust Homepage]]