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
