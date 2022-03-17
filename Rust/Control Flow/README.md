# Control Flow

## if Expressions

- An `if` expression allows to branch the code depending on conditions.

    ```Rust
    let number = 5;
    if number < 5 {
        println!("true");
    } else {
        println!("false");
    }
    ```

- Branching with `if-else` is similar to other languages. However, the boolean condition doesn't need to be surrounded by parentheses, and each condition is followed by a block.
- All branches must return the same type.
- Note: the condition must be a `bool`. If the condition isn't a `bool`, an error will occur.

    ```Rust
    let number = 3;
    if number {
        println!("true");
    }
    // error[E0308]: mismatched types
    ```

- Using *if* in a *let* statement:
  - Since `if` is an expression, it can be used on the right side of a `let` statement to assign the outcome to a variable.

    ```Rust
    let condition = true;
    let number = if condition { 5 } else { 6 };
    // result in number = 5
    ```

## Looping

- Rust has 3 kinds of loops: `loop`, `while`, `for`.

***Loop***

- The `loop` keyword tells Rust to indicate an infinite loop.

    ```Rust
    let mut count = 0u32;
    loop {
        count += 1;
        if count == 3 {
            println!("three");
            // skip the rest of this iteration
            continue;
        }

        println!("{}", count);
        if count == 5 {
            println!("enough");
            // exit this loop
            break;
        }
    }
    ```

- The `break` statement can be used to exit a loop at any time, whereas the `continue` statement can be used to skip the rest of the iteration and start a new one.
- Nesting and labels: it is possible to `break` or `continue` outer loops whel dealing with nested loops.

    ```Rust
    'outer: loop {
        println!("Entered the outer loop");
        'inner: loop {
            println!("Entered the inner loop");
            // breaks the outer loop
            break 'outer;
            // or breaks the inner loop with `break;`
        }
    }
    println!("Exited the outer loop");
    ```

***Returning values from loops***

- One of the uses of a `loop` is to retry an operation until it succeeds.
- To pass the result of an operation out of the loop, add the value want to return after the `break` expression used to stop the loop; that value will be returned out of the loop.

    ```Rust
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    }
    ```

***Conditional loops with while***

- The `while` keyword can be used to run a loop while a condition is true.

    ```Rust
    let mut number = 3;
    while number != 0 {
        println!("{}", number);
        number -= 1;
    }
    ```

- This construct eliminates a lot of nesting that would be necessary if used `loop`, `if`, `else`, and `break`, and it's clearer.

***Looping through a collection with for***

- for and range:
  - The `for in` construct can be used to iterate through an `Iterator`.
  - One of the easiest ways to create an iterator is to use the range notation `a..b`.
    - This yields values from `a` (include) to `b` (exclude) in steps of 1.

    ```Rust
    for n in 1..101 {
        if n % 2 == 0 {
            println!("even");
        } else {
            println!("odd");
        }
    }
    ```

    - Alternatively, `a..=b` can be used for a range that inclusive on both ends.
- for and iterators:
  - The `for in` construct is able to interact with an `Iterator` in serveral ways.
  - By default, the `for` loop will apply the `into_iter` function to the collection. However, this is not the only means of converting collection into iterators.
    - `iter`: borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.
  
        ```Rust
        let names = ["a", "b", "c"];
        for name in names.iter() {
            match name {
                &"a" => println!("This is the one");
                _ => println!("Hello normal one: {}", name);
            }
        }
        println!("names: {:?}", names);
        ```

    - `into_iter`: consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed, it is no longer available for reuseas it has been 'moved' within the loop.

        ```Rust
        let names = ["a", "b", "c"];
        for name in names.into_iter() {
            match name {
                &"a" => println!("This is the one");
                _ => println!("Hello normal one: {}", name);
            }
        }
        println!("names: {:?}", names);
        // error[E0382]: borrow of moved value: `names`
        ```

    - `iter_mut`: mutably borrows each element of the collection, allowing for the collection to be modified in place.

    ```Rust
    let mut names = vec!["Bob", "Frank", "Ferris"];
    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }
    println!("names: {:?}", names);
    // result in names: ["Hello", "Hello", "There is a rustacean among us!"]
    ```

## Match

- Rust provides pattern matching via the `match` keyword, which can be used like a C `switch`.
- The first matching arm is evaluated and all possible values must be covered.

    ```Rust
    let number = 13;
    match number {
        // match a single value
        1 => println!("One!"),
        // match several values
        2 | 3 | 4 | 5 => println!("Random"),
        // match an inclusive rante
        13..=19 => println!("Some other random"),
        // handle the rest of cases
        _ => println!("Ain't special"),
    }
    // result in Some other random
    ```

- `match` is an expression too.

    ```Rust
    let boolean = true;
    let binary = match boolean {
        // the arms of a match must cover all the possible values
        false => 0,
        true => 1,
    };
    println!("{} -> {}", boolean, binary);
    // result in true -> 1
    ```
