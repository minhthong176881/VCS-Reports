# Modules and Cargo

## Modules

- Rust provides a powerful module system that can be used to hierarchically split code in logical units (modules), and manage visibility (public/private) between them.
- A module is collection of items: functions, structs, trais, `imp` block, and even other modules.
- A keyword `mod` is used to define a module as a block.

    ```Rust
    mod foo {
        struct Foo {
            s: &'static str // private 
        }
    }

    fn main() {
        let f = foo::Foo{s: "hello"};
        println!("{:?}", f);
    }

    // error: struct Foo is private
    ```

- By default, the struct is made private. To choose what items to make public from a module, use `pub` keyword. The set of functions and types exported from a module is called its *interface*.

    ```Rust
    mod foo {
        pub struct Foo {
            s: &'static str
        }

        impl Foo {
            pub fn new(s: &'static str) -> Foo {
                Foo{s: s}
            }
        }
    }

    fn main() {
        let f = foo::Foo::new("hello");
        println!("{:?}", f);
    }

    // result in hello
    ```

- Within a module, all items are visible to all other items.
- The `use` declaration can be used to bind a full path to a new name, for easier access.
- The `as` keyword can be used to bind imports to a different name.

    ```Rust
    use crate::deeply::nested::{
        my_first_function,
        my_second_function,
        AndATraitType
    };
    use deeply::netsted::function as other_function;

    fn main() {
        my_first_function();
        other_function();
    }
    ```

- The `super` and `self` keywords can be ued in the path to remove ambiguity ưhen accessing items and to prevent unnecessary hadcoding of paths.
  - The `super` keyword refers to the parent scope (outside the current module).
  - The `self` keyword refers to the current module scope.

## Crates

- A crate is a compilation unit in Rust.
- Whenever `rustc some_file.rs` is called, `some_file.rs` is treated as the *crate file*.
- A crate can be compiled into a binary or into a library. By default, `rustc` will produce a binary from a crate.

    ```Rust
    // some_file.rs
    pub fn some_function() {
        println!("Hello, world!");
    }
    ```

    ```text
    $ rustc --crate-type=lib some_file.rs
    $ ls lib*
    libsome_file.rlib
    ```

- To link a crate to this new library, use `--extern` flag.

    ```Rust
    // executable.rs
    fn main() {
        some_file::some_function(); // use some_file lib
    }
    ```

    ```text
    $ rustc executable.rs --extern some_file=libsome_file.rlib && ./executable
    Hello, world!
    ```

## Cargo

- `cargo` is the official Rust package management tool. It has lots of really usefull features to improve code quality and developer velocity.
- Cargo does 4 things:
  - Introduce 2 metadata files with various bits of package information.
  - Fetches and builds the package's dependencies.
  - Invokes `rustc` or another build tool with the correct parameters to build the package.
  - Introduces conventions to make working with Rust packages easier.
- To create a new Rust project: `cargo new <project_name>`.
- File hierarchy:

    ```text
    <project_name>
    ├── Cargo.toml
    └── src
        └── main.rs
    ```

  - The `main.rs` is the root source file for the project.
  - The `Cargo.toml` is the config file for `cargo` for this project.

- To add a dependency to the program, add the dependency under `[dependencies]` in the `Cargo.toml` file.

    ```text
    [dependencies]
    serde = "1.0"
    ```

- To build a project, execute `cargo build` anywhere in the project directory.
- Do `cargo run` to build and run.
- The complied files are located in `target/debug/` directory. To put the resulting binary in `target/release/` directory, use `--release` flag.
  - Compiling in debug mode is the default for development. Compilation time is shorter since the compiler doesn't do optimizations, but the code will run slower.
  - Release mode takes longer to compile, but the code will run faster.
- Update dependencies:

    ```text
    $ cargo update                      // updates all dependencies
    $ cargo update -p <dependency_name> // updates just a dependency
    ```
