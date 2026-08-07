Similarly to other programming languages, Rust allows control flow via if statements, however, what Rust does differently is that in an if statement, the expression *must* always evaluate to a boolean.

```rust
let is_true: bool = true;

if is_true {
	// Works just fine
}
```

However, if your if statement's inner expression does not evaluate to a boolean, then the Rust compiler will throw an error, for example:

```rust
i32

mismatched types
expected `bool`, found integer rustc(E0308)

main.rs(5, 8): Exact error occurred here
```

A fairly nice feature Rust offers is inline if statements.

```rust
let condition: bool = true;
let number: i32 = if condition { 1 } else { 2 };
```

---

There are a number of ways to create loops in Rust as well, with the most common way being `loop`.

```rust
fn main() {
	loop {
		// Statement or Expression
	}
}
```

```rust
fn main() {
	let mut counter = 0;
	let result = loop {
		counter += 1;
		
		if counter == 10 {
			break counter; // Stop and return the value.
		}
	}
}
```

This kind of loop will run indefinitely unless you supply a `break` to stop it.

Another common loop format seen is the while loop, which runs until a condition is met.

```rust
fn main() {
	let mut number = 3;
	
	while number != 0 {
		println!("Number: {number}");
		number -= 1;
	}
	
	println!("Done!");
}
```

The final type of loop is called a for loop, which is useful when you want to iterate over a collection of items.

```rust
fn main() {
	let my_array = [10, 20, 30, 40, 50];
	
	for element in a.iter() { // Type of "element": &i32
		println!("Number: {element}")
	}
	
	// Creating a range from 1-3
	
	for number in 1..4 {
		println!("Range number: {number}");
	}
}
```
