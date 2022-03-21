# Functions

- The most important functions in the language: the `main` function, which is the entry point of many programs.
- `fn` keyword is used to declare new functions.
- Rust code use *snake case* as the conventional style for function and variable names.

    ```Rust
    fn main() {
        println!("Hello world!");
        another_function();
    }

    fn another_function() {
        println!("Another function!");
    }
    ```

- A function is defined by entering `fn` followed by a function name and a set of parentheses. The curly brackets tell the compiler where the function body begins and ends.
- A function definded can be called by entering its name follwed by a set of parentheses.

## Parameters

- Parameters are special variables that are part of a function's signature.

    ```Rust
    fn main() {
        another_function(5);
    }

    fn another_function(x: i32) {
        println!("The value of x is: {}", x);
    }
    ```

- In function signature, the type of each parameter must be declared.
- When defining multiple parameters, separate the parameter declarations with commas.

    ```Rust
    fn multiple_param_function(x: i32, c: char) {
        println!("{}{}", x, c);
    }
    ```

## Functions with return values

- Functions can return values to the code that calls them.
- Don't name return values, but must declare their type after an arrow (`->`).
- The return value of the function is synonymous with the value of the final expression the the bock of the body of a function.
- Return early from a function by using the `return` keyword and specifying a value.

    ```Rust
    fn five() -> i32 {
        5
    }

    fn main() {
        let x = five();
        println!("{}", x); // return in 5
    }
    ```

- Note: there is no semicolon at the end of returning expression. If placing a semicolon at the end of expression line, the return value will be come `()`.

    ```Rust
    fn main() {
        let x = plus_one(5);
        println!("{}", x);
    }

    fn plus_one(x: i32) -> i32 {
        x + 1; // <- error here
    }

    // error[E0308]: mismatched types
    ```

## Associataed functions & Methods

- Some function are connected to a particular type. These come in 2 forms: assosicated functions, and methods.

***Method***

- Methods are similar to functions: declared with the `fn` keyword and a name, can have parameters and return value...
- Methods are defined within the context of a struct (or an enum), and their first parameter is always `self`, which represents the instance of the struct the method is being called on.

    ```Rust
    struct Rectangle {
        width: u32,
        height: u32,
    }

    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }
    }

    fn main() {
        let rect1 = Rectangle {
            width: 30,
            height: 50,
        };
        println!("Area = {}", rect1.area()); // result in Area = 1500
    }
    ```

- To define a function within the context of `Rectangle`, start an `imp` (implementation) block for `Rectangle`.
  - Everything within this `imp` block will be associated with the `Rectangle` type.
  - Define the `area` function within the `imp` and set the first parameter to be `self`.
  - Call the `area` method by using *method syntax*: add a dot followed by the method name, parentheses, and any arguments.

***Associated Functions***

- All functions defined within an `impl` block are called *associated functions* because they're associated with the type named after the `impl`.
- Associated functions can be defined without `self` as the first parameter.
- Associated functins are often used for constructors that will return a new instance of the struct.

    ```Rust
    impl Rectangle {
        fn square(size: u32) -> Rectangle {
            Rectangle {
                width: size,
                height: size,
            }
        }
    }
    ```

- To call a associated function, use the `::` syntax with the struct name: `let sq = Rectangle::square(3);`.
