Rust has 4 main Scalar data types: Integers, Floating-point Numbers, Booleans, and Characters.

| Length  | Signed        | Unsigned      |
| ------- | ------------- | ------------- |
| 8-bit   | ***`i8`***    | ***`u8`***    |
| 16-bit  | ***`i16`***   | ***`u16`***   |
| 32-bit  | ***`i32`***   | ***`u32`***   |
| 64-bit  | ***`i64`***   | ***`u64`***   |
| 128-bit | ***`i128`***  | ***`u128`***  |
| arch    | ***`isize`*** | ***`usize`*** |

Alongside various sizes of integers, we can also represent data types in various formats.

```rust
let a: i32 = 98_222; // Decimal
let b: i32 = 0xff; // Hex
let c: i32 = 0o77; // Octal
let d: i32 = 0b1111_0000; // Binary
let e: i32 = b'A'; // Byte (u8 only)
```

If you set a value to one higher than its designated integer limit (i.e., 256 for an 8-bit integer), Rust will (in release mode) perform a two's complement wrapping, which will bring it down back to 0.

However, in debug mode, the Rust compiler will simply just panic.

---

Characters are written similarly to languages like C++, in single-quotes, represented with the type: ***`char`***.

```rust
let a: char = 'a';
let my_char: char = '!';
```

Next, we have tuples. Similarly to Python, they compound 2 data values into 1 type, useful for storing a key and a value.

We can access tuple values directly using ***`.`***, and we can even create individual values as shown below, so we can then print ***`channel`*** and ***`sub_count`*** respectively.

```rust
let my_tuple: (&str, i32) = ("My YouTube Channel", 300_000);

let (channel: &str, sub_count: i32) = my_tuple;

let sub_count: i32 = my_tuple.1; // Index, 0-based
```

To declare arrays in Rust, we can use a standard comma-separated syntax wrapped in brackets (***`[ ... ]`***), with them of course using 0-based indexing.

```rust
let error_codes: [i32; _] = [200, 404, 500];

let not_found: i32 = error_codes[1]; // 404
```

Arrays are statically sized, which means their size cannot change, unlike modern languages such as JavaScript or Python. Their size correlates with their original declaration.

If we were to try and access an index that is outside the range of the array, we'd get a compile-time error, like so:

```rust
let my_array: [i32; _] = [1, 2, 3];

println!("{}", my_array[3]); // Attempts to access the 4th element.
```

Attempting this will cause the program to not compile, and this happens due to the explicit sizing of the array that we declared.

Let's focus more on this:

```rust
let byte: [i32; _] = [0; 8];
```

This is telling the compiler that we want to create an array with 8 values, all set to 0.
