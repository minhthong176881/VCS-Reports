# Parsing with Nom

- Nom is a parser library for Rust. Its goal is to provide tools to build safe parsers without compromising the speed of memory consumption.
- Nom is macros and Nom prefers to work with byte slices, not strings (can be used for any data format, not just text).
- Nom is used to decode binary protocols and file headers. It can also work with 'text' in encodings other than UTF-8.
- Nom is *not* based on grammars or generators, nom parsers are simple rust functions.
- All nom parsers fullfil the parser trait, which describes the execution of a single parse.

    ```Rust
    trait Parser<I, O, E> {
        fn parse(&mut self, input: I) -> IResult<I, O, E>;
        ...
    }
    ```

  - The `IResult` type:
    - Encodes the result of a parser.
    - There are 3 possibilities:
      - `Done` - success - get both the result and the remaining bytes.
      - `Error` - failed to parse - get an error.
      - `Imcomplete` - more data needed.
- A basic parser:

    ```Rust
    fn parse_hello(input: &str) -> IResult<&str, &str, ()> {
        match input.strip_prefix("hello") {
            Some(tail) => Ok((tail, "hello")),
            None => Err(nom::Err::Error(())),
        }
    }
    ```

- Parser combinators: instead of writing the grammar in a separate syntax and generating the corresponding code, use smaller functions with specific purposes, such as "take 5 bytes", or "recognize the word 'HTTP'", and assemble them in meaningful pattern like "recognize 'HTTP', then space, then a version".
  - Advantage:
    - The parsers are small and easy to write.
    - The parsers components are easy to reuse.
    - The parsers components are easy to test separately.
    - The parser combination code looks close to the grammar.
- Making new parsers with function combinators:
  - Nom is based on functions that generate parsers, with signature: `(arguments) -> impl Fn(Input) -> IResult<Input, Output, Error>`.
  - The arguments of a combinator can be direct values or even other parsers.

    ```Rust
    use nom::{
        IResult,
        bytes::streaming::take,
        sequence::delimited,
        character::complete::char,
        bytes::complete::is_not
    };

    fn parens(input: &str) -> IResult<&str, &str> {
        // delimited parser takes other parsers (char, is_not) as arguments
        delimited(char('('), is_not(")"), char(')'))(input)
    }
    fn take4(input: &str) -> IResult<&str, &str> {
        // take parser take a string as the argument
        take(4u8)(input)
    }
    ```

- Combining parsers:
  - There are higher level patterns, like `alt` combinator, which provides a choice between multiple parsers. If one branch fail, it tries the next, and returns the result of the first parser that succeeds.
  - Some other basic combinators available:
    - `opt`: wll make the parser optional (if it return the `O` type, the new parser returns `Option<O>`).
    - `many0`: will apply the parser 0 or more times (if it return the `O` type, the new parser returns `Vec<O>`).
    - `many1`: will apply the parser 1 or more times.
    - `tuple`: used to apply a series of parsers then assemble their results.
  - Sequence of combinators written in imperative style using `?` operator:

    ```Rust
    use nom::{IResult, bytes::complete::tag};

    struct A {
        a: u8,
        b: u8
    }

    fn ret_int1(i: &[u8]) -> IResult<&[u8], u8> { Ok((i, 1)) }
    fn ret_int2(i: &[u8]) -> IResult<&[u8], u8> { Ok((i, 2)) }

    fn f(i: &[u8]) -> IResult<&[u8], A> {
        // if successful, the parser returns `Ok((remaining_input, output_value))`
        let (i. _) = tag("abcd")(i)?;
        let (i, a) = ret_int1(i)?;
        let (i, _) = tag("efgh")(i)?;
        let (i, b) = ret_int2(i)?;

        Ok((i, A { a, b }))
    }

    let r = f(b"abcdefghX");
    assert_eq!(r, Ok((&b"X"[..], A{ a: 1, b: 2 })));
    // result in Ok
    ```

- Streaming/Complete:
  - Some of nom's modules have `streaming` or `complete` submodules. They hold different variants of the same combinators.
  - A streaming parser assumes that we might not have all of the input data. This can happen with some network protocol or large file parsers, where the input buffer can be full and need to be resized or refilled.
  - A complete parser assumes that we already have all of the input data. This will be the common case with small files that can be read entirely to memory.

    ```Rust
    fn take_streaming(i: &[u8]) -> IResult<&[u8], &[u8]> {
        bytes::streaming::take(4u8)(i)
    }
    fn take_complete(i: &[u8]) -> IResult<&[u8], &[u8]> {
        bytes::complete::take(4u8)(i)
    }
    // both parsers will take 4 bytes as expected
    // if the input is smaller than 4 bytes, the streaming parser will return `Incomplete` to indeicate that need more data
    // but the complete parser will return error.
    ```
