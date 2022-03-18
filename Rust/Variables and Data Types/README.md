# Variables and Data Types

## Variables

- Variables are declared with `let` keyword.
- By default variables are immutable - once a value is bound to a name, cannot change that value.

    ```Rust
    let x = 5;
    x = 6;
    // error: cannot assign twice to immutable variable `x`
    ```

- Variable can be mutable by adding `mut` in front of the variable name.

    ```Rust
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
    // result: 
    //      The value of x is: 5
    //      The value of x is: 6
    ```

- It's possible to declare variable bindings first, and initialize them later. However, this form is seldom used, as it may lead to the use of uninitialized variables.

    ```Rust
    let a;
    a = 2 * 2;
    ```

- When data is bound by the same name immutably, it also *freezes*. *Frozen* data can't be modified until the immutable binding goes out of scope.

    ```Rust
    let mut mutable_integer = 20;
    {
        // shadowing by immutable `mutable_integer`
        let mutable_integer = mutable_integer;

        // error: `mutable_integer` is frozen in this scope
        mutable_integer = 50;
    }

    // ok: `mutable_integer` goes out of scope and is not frozen
    mutable_integer = 3;
    ```

***Constants***

- Like immutable variables, *constants* are values that are bound to a name and are not allowed to change.
- Differences between constants and variables:
  - `mut` is not allowed to use with constants. Constants aren't just immutable by default, they are **always** immutable.
  - Constants are declared using the `const` keyword instead of the `let` keyword, and the type of the value *must* be annotated.
  - Constants can be declared in any scope, including the global scope.
  - Constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

    ```Rust
    const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
    ```

    - Rust's naming convention for constants is to use all uppercase with underscores between words.
  
***Scope***

- Variable bindings have a scope, and are constrained to live in a *block*.
- A block is a collection of statements eclosed by braces `{}`.

    ```Rust
    fn main() {
        let outer_var = 1;
        // this is a block, and has a smaller scope than the main function
        {
            let inner_var = 2;
        }
        // end of block

        // error! `inner_var` doesn't exist in this scope
        println!("inner scope variable: {}", inner_var);
    }
    ```

***Shadowing***

- A new variable can be declared with the same name as a previous variable. The first variable is called is *shadowed* by the second -> the second variable's value is what the program sees when the variable is used.
- To shadow a variable, use the same variable's name and repeating the use of the `let` keyword.

    ```Rust
    let x = 5;
    let x = x + 1;
    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {}", x);
    }
    println!("The value of x is: {}", x);
    // result:
    //      The value of x in the inner scope is: 12
    //      The value of x is: 6
    ```

- Shadowing is different from marking a variable as `mut`. Compile-time error will occur if try to reassign to this variable without using the `let` keyword.
- Shadowing can change the type of the value but reuse the same name.

    ```Rust
    let spaces = "    ";
    let spaces = spaces.len();
    // the first "space" variable is a string type and the second is number type
    ```

  - However, compile-time error will occur if try to use `mut` for this.

    ```Rust
    let mut spaces = "    ";
    spaces = spaces.len();
    // error[E0308]: mismatched types
    ```

## Data Types

- Every value in Rust is of a certain *data type*.
- Rust is a *statically typed* language, which means that it must know the types of all variables at compile time.
- The compiler can usually guess what type of the variable based on the value.
- In case when many types are possible, such as when converting a `String` to a numeric type using `parse`, must add a type annotation.

    ```Rust
    let guess: u32 = "42".parse().expect("Not a number");

    // if it doesn't have the type annotation, an error will occur:
    //      error[E0282]: type annotations needed
    ```

### Scalar Types

- A *scalar* type represents a single value.

***Integer Types***

- An *integer* is a number without a fractional component.
- Integer Types in Rust:

    Length | Signed | Unsigned
    --- | --- | ---
    8-bit | `i8` | `u8`
    16-bit | `i16` | `u16`
    32-bit | `i32` | `u32`
    64-bit | `i64` | `u64`
    128-bit | `i128` | `u128`
    arch | `isize` | `usize`

  - Signed numbers are stored using *two's complement* representation.
  - Each signed variant can store numbers from -(2^(n-1)) to 2^(n-1) - 1 inclusive, where *n* is the number of bits that variant uses. (i.e. `i8` can store number from -(2^7) to 2^7 - 1).
  - Unsigned variants can store numbers from 0 to 2^n - 1.
  - The `isize` and `usize` types depend on the architecture of the computer (“arch”: 64 bits if on a 64-bit architecture and 32 bits if on a 32-bit architecture).
  - Integer types default is `i32`.
- A numeric literal is a character string selected from the digits, the plus sign, the minus sign, and the decimal point...
- Integer literals in Rust:

    Number literals | Example
    --- | ---
    Decimal | `98_222`
    Hex | `0xff`
    Octal | `0o77`
    Binary | `0b1111_0000`
    Byte (`u8` only) | `b'A'`

  - Numeric literals can be type annotated by adding the type as a suffix (i.e. to specify that the literal `42` should have the type `i32`, write `42i32`).
  - Numberic literals can also use `_` as a visual separator to make the number easier to read, such as `1_000` will have the same value as `1000`.

***Floating-Point Types***

- Rust's floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respectively.
- The default type is `f64`.
- All floating-point types are signed.

    ```Rust
    let x = 2.0; // f64
    let y : f32 = 3.0; // f32
    ```

***Numeric Operations***

- Rust supports the basic mathematical operations: addition, subtraction, multiplication, division, and remainder.
- Integer division rounds down to the nearest integer.

    ```Rust
    // adition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let floored = 2 / 3; // results in 0

    // remainder
    let remainder = 43 % 5;
    ```

- See list of all operators that Rust provides [here](https://doc.rust-lang.org/book/appendix-02-operators.html).

***The Boolean Type***

- A boolean type in Rust has 2 possible values: `true` and `false`.
- Booleans are 1 byte in size.
- The Boolean type is specified using `bool`.

    ```Rust
    let t = true;
    let f: bool = false;
    ```

***The Character Type***

- `char` literals are specified with single quoutes, as opposed to string literals, which use double quoutes.
- Rust's `char` type is 4 bytes in size and represents a Unicode Scalar Value, which meaans it can represent a lot more than just ASCII (accented letters; Chinese, Japanese, Korean characters; emoji; and zero-width spaces).

### Compound Types

- Compound types can group multiple values into one type.

***The Tuple Type***

- A tuple is a general way of grouping together a number of values with a variety of types into one compound type.
- Tuples have a fixed length: once declared, they cannot grow or shrink in size.
- Create a tuple by writing a comma-separated list of values inside parenthesis. Each position in the tuple has a type, and the types of the different values in the tuple don't have to be the same.

    ```Rust
    let tup: (i32, f64, u8) = (500, 6.5, 1);
    ```

- *Destructuring*: use a pattern with `let` to turn a tuple into separate variables.

    ```Rust
    let tup = (500, 6.5, 1);
    let (x, y, z) = tup;
    println!("The value of y is: {}", y); // result in 6.5
    ```

- A tuple element also can be accessed directly by using a period (`.`) followed by the index of the value. The first index in a tuple is 0.

    ```Rust
    let x: (i32, f64, u8) = (500, 6.5, 1);
    let first = x.0;
    let second = x.1;
    let third = x.2
    ```

- The tuple without any values. `()`, is a special type that has only one value. The type is called the *unit type* and the value is called the *unit value*. Expressions return the unit value if they don't return any other value.

***The Array Type***

- Every element of an array must have the same type.
- Arrays in Rust have a fixed length.
- The values in an array are written as a comma-separated list inside square brackets.

    ```Rust
    let a = [1, 2, 3, 4, 5];
    ```

- Other way to initialize an array:
  - Write an array's type using square brackets with the type of each element, a semicolon, and then the number of elements in array.

    ```Rust
    let a: [i32; 5] = [1, 2, 3, 4, 5];
    ```

  - Initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in squar brackets/

    ```Rust
    let a [3; 5];
    // same as a = [3, 3, 3, 3, 3];
    ```

- Array elements can be accessed by using indexing.

    ```Rust
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    let second = a[1];
    ```

- Cannot print out an array in the usual way with `{}` but can do a *debug* print with `{:?}`.

    ```Rust
    let ints = [1, 2, 3, 4];
    println!("ints: {:?}", ints);
    // result in ints [1, 2, 3, 4]
    ```

***Vectors***

- Vectors are *re-sizeable* arrays and behave much like Python `List` and C++ `std::vector`.
- Vectors allow to store more than one value in a single data structure that puts all the values next to each other in memory.
- Vectors can only store values of the same type.
- Creating a new vector:
  - Call the `Vec::new` function.
  
    ```Rust
    // add a type annotation here if don't insert any values to this vector
    let v: Vec<i32> = Vec::new();
    ```

  - Use the `vec!` macro:

    ```Rust
    let v = vec![1, 2, 3];
    ```

- Updating a vector:
  - Add elements to a vector using the `push` method.

    ```Rust
    let mut v = Vec::new();
    v.push(5);
    v.push(6);
    v.push(7);
    ```

- Reading elements of vectors:
  - There are 2 ways to reference a value stored in a vector.

    ```Rust
    let v = vec![1, 2, 3, 4];
    let third: &i32 = &v[2];
    println!("The third element is {}", third),
    match v.get(2) {
        Some(third) => println!("The third element is {}", third),
        None => println!("There is no third element.");
    }
    ```

    - First, use index value of `2` to get the third element: vectors are indexed by number, starting at zero, using `&` and `[]` which gives a reference.
    - Second, use the `get` method with the index passed as an argument.

### Cutom Types

***Structures***

- Structs are similar to tuples in that both hold multiple related values.
- The pieces of a struct can be different types.
- In a struct, each piece of data is named so it's clear what the values mean.
- To define a struct, enter the keyword `struct` and name the entire struct. Then, inside curly brackets, define the names and types of the pieces of data, which called *field*.

    ```Rust
    struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
    }
    ```

- To use a struct, create an *instance* of that struct by specifying concrete values for each of the field.
- To get a specific value from a struct, use dot notation. If the instance is mutable, the value can be changed by using the dot notation and assigning into a particular field.

    ```Rust
    // create an instance of struct User
    let mut user1 = User {
        email: String::from("somemail@mail.com"),
        username: String::from("somename"),
        active: true,
        sign_in_count: 1,
    };
    // change email of the instance
    user1.email = String::from("newmail@mail.com");
    ```

- Note: the entire instance must be mutable, cannot mark only certain fields as mutable.
- Create new instances from other instances:
  - Use struct update syntax `..` that specifies the remaining fields should have the same value as the fields in the given instance.

    ```Rust
    let user2 = User {
        email: String::from("mail2@mail.com"),
        ..user1
    }
    ```

- Rust also supports structs that look similar to tuples, called *typle structs*.
  - To define a tuple struct, start with the `struct` keyword and the struct name followed by the types in the tuple.

    ```Rust
    struct Color(i32, i32, i32)
    struct Point(i32, i32, i32)

    fn main() {
        let black = Color(0, 0, 0);
        let origin = Point(0, 0, 0);
    }
    ```

- Structs that don't have any fields are called *unit-like structs* because they behave similarly to `()`.

    ```Rust
    struct AlwaysEqual;

    fn main() {
        let subject = AlwaysEqual;
    }
    ```

***Enums***

- Enums are a way of defining custom data types in a different way with structs.
- Enums used when all possible variants are known and can be enumerated.

    ```Rust
    enum IpAddrKind {
        V4,
        V6,
    }
    // V4 and V6 are variants of the enum
    // IpAddrKind is now a custom data typs that can be used elsewhere in the code
    ```

- Create instances:

    ```Rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;

    fn route(ip_kind: IpAddrKind) {}
    ```

- Associate type to variant and put data into each enum variant:

    ```Rust
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
    ```
