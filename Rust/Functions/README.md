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