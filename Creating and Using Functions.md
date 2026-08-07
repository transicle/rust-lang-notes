Rust uses a simple ***`snake_case`*** style for function names.

Similarly to variables, the Rust compiler can infer what you are intending to do with types if specified properly.

```rust
fn main() {
	my_func(1, 2);
}

fn my_func(
	x: i32,
	y: 32
) {
	println!("Another function!");
	
	println!("X: {x}, Y: {y}");
}
```

In Rust, we can conceptualize a piece of code as either a statement, or an expression, whereas statements perform some action but do *not* return a value, and expressions *do* return a value.

We are able to create functions that return a value a few different ways in Rust, using either implicit or explicit returns.

An implicit return is where: On the last line in a function implementation, you omit the semicolon and the return keyword, leaving just what you want to return, an expression. The type of what you want to return, *must* match the return type of the function itself.

```rust
fn main() {
	let x: i32 = my_func();
	
	println!("X: {x}");
}

fn my_func() -> i32 {
	let x = 1;
	let y = 2;
	
	x + y // Implicitly stating: return x + y;
}
```

