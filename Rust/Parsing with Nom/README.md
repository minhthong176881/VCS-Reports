# Parsing with Nom

- Nom is a parser library for Rust. Its goal is to provide tools to build safe parsers without compromising the speed of memory consumption.
- Nom is macros and Nom prefers to work with byte slices, not strings (can be used for any data format, not just text).
- Nom is used to decode binary protocols and file headers. It can also work with 'text' in encodings other than UTF-8.