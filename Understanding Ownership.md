**Rules of Ownership**:

1. Each value has a variable that's called its owner.
2. There can *only* be one owner at a time.
3. When the owner goes out of scope, the value of the variable's dropped.

**Rules of References**:

1. At any given time, you can have *either* one mutable reference, or any number of immutable references.
2. References must always be valid.

---

Due to Rust being a systems language, it's highly important to understand how our memory is laid out during runtime.

During runtime, our program has access to *both* the stack and the heap, where the stack has a static size, with it being unable to grow/shrink during runtime. The stack also stores stack frames which are created for every function that executes, and the stack frames stores the local variables of the function being executed. The variables inside of stack frames must have a dedicated, known size at compile time too.

Unlike the stack, the heap is dynamically allocated and can grow/shrink at runtime. It's less organized and can be large or smaller amounts, and *we* control the lifetime of the data.

---

```rust
fn main()
{
	let x = 5;
	let y = x; // Copy
	
	let s1 = String::from("Hello"); // Allocated on the heap
	let s2 = s1; // Move (not a shallow copy)
	
	println!("{} world!", s1);
}
```

The code about would error because of the following: ***`value borrowed here after move (s1)`***. Since we're defining ***`s2`*** to be ***`s1`***, ***`s1`***'s position is being changed.

To write this code properly, we need to call a clone, which creates a copy of this, allowing us to be able to use ***`s1`*** and ***`s2`*** effectively.

```rust
fn main()
{
	...
	
	let s1 = String::from("Hello"); // Allocated on the heap
	let s2 = s1.clone();
	
	...
}
```

---

An important thing to note, is when you pass a parameter into a function, it's essentially the same as assigning a value to another variable.

```rust
fn main()
{
	let s = String::from("Hello");
	
	takes_ownership(s);
	
	println!("{}", s); // Line 7
}

fn takes_ownership(
	some_string: String
) {
	println!("{}", some_string);
}
```

This code is going to error. The reason this fails is because when we call ***`takes_ownership`*** and provide ***`s`***, we transfer the ownership directly over to the function we're calling. After transferring ownership (a.k.a. a move operation), we're unable to borrow that value again and the error on line 7 appears.

With integers it's different, instead of transferring ownership, integers create a copy. So the same code but on an integer *will* succeed because it replicates itself rather than transferring who it belongs to.

This also works the opposite way, we can define a variable, let's say ***`s1`*** that is equal to the return value of the function: ***`gives_ownership`***.

```rust
fn main()
{
	let s1 = gives_ownership();
	
	println!("{}", s1);
}

fn gives_ownership() -> String
{
	let some_string = String::from("Hello, world!");
	
	some_string
}
```

What's different about this is since we're *returning* a value in ***`gives_ownership`***, ***`some_string`***'s ownership is being moved (or transferred) over to the ***`s1`*** variable, allowing us to use it afterwards.

Lastly, we're also able to take ownership and give it back to the original owner. Using context from the previous examples, the following *should* be self-explanatory:

```rust
fn main()
{
	let s1 = gives_ownership();
	let s2 = String::from("Hello, world!");
	let s3 = takes_and_gives_back(s2);
	
	println!("s1:{}, s3:{}", s1, s3);
}

fn gives_ownership() -> String
{
	let some_string = String::from("Hello, world!");
	
	some_string
}

fn takes_and_gives_back(
	a_string: String
) -> String {
	a_string
}
```

---

```rust
fn main()
{
	let s1 = String::from("Hello, world!");
	let (s2, len) = calculate_length(s1);
	
	println!("The length of \"{}\" is {}.", s2, len);
}

fn calculate_length(
	s: String
) -> (String, usize) {
	let length = s.len();
	
	(s, length)
}
```

The example above is clunky and not really idiomatic code, references can actually fix this:

```rust
fn main()
{
	let s1 = String::from("Hello, world!");
	let len = calculate_length(&s1);
	
	println!("The length of \"{}\" is {}.", s1, len);
}

fn calculate_length(
	s: &String
) -> usize {
	let length = s.len();
	
	length
}
```

This works a lot cleaner due to us providing a *reference* to a String, rather than transferring the ownership itself.

Passing in references as function parameters is called *borrowing*, it's called that because we borrow the value without actually taking ownership of the argument. Another thing to keep in mind is that they are also immutable by default.

```rust
fn main()
{
	let s1 = String::from("Hello, ");
	
	change(&s1);
}

fn change(
	some_string: &String
) {
	some_string.push_str("world");
}
```

This will not work, due to the parameter being immutable by default.

However, if we *do* want to modify the function's parameter like this, we can change it in a few ways.

```rust
fn main()
{
	let mut s1 = String::from("Hello, ");
	
	change(&mut s1);
}

fn change(
	some_string: &mut String
) {
	some_string.push_str("world");
}
```

The change function is now able to take in and modify the function's parameter, because we're essentially borrowing the mutability of the variable.

Mutable reference do have a big restriction: You can only have one mutable reference to a particular piece of data to a particular scope.

```rust
fn main()
{
	let mut s = String::from("Hello, world!");
	
	let r1 = &mut s;
	let r2 = &mut s; // Cannot borrow `s` as mutable
					 //  more than once at a time.
	
	println!("{}, {}", r1, r2);
}
```

**TL***;**DR**: The problem this *would* cause if this were possibly is that we could read a value at the same time as something changes the value of it, which is dangerous.

The *best* way to fix this problem is to remove the mutability directly.

```rust
fn main()
{
	let s = String::from("Hello, world!");
	
	let r1 = &s;
	let r2 = &s;
	
	println!("{}, {}", r1, r2);
}
```

Rust actually *does* let you do this, but only after the scope of the immutable variables end.

If you declare a variable as mutable, and create immutable references towards them, you cannot create a mutable reference until the last time the immutable references are referenced.

```rust
fn main()
{
	let mut s = String::from("Hello, world!");
	
	let r1 = &s;
	let r2 = &s;

	println!("{}, {}", r1, r2);

	let r3 = &mut s;
	
	println!("{}", r3);
}
```

---

**Dangling references**: What happens if we have a reference that points to invalid data?

```rust
fn main()
{
	let reference_to_nothing: dangle();
}

fn dangle() -> &String
{
	let s = String::from("Hello, world!");
	
	&s
}
```

At first glance this appears normal, however the return type of this function, ***`&String`*** must be able to be accessed after the scope of the function has been closed. In this example, it doesn't do that.

The variable ***`s`*** is declared in the same scope as it's returned so Rust will automatically de-allocate this for us, therefore, ***`reference_to_nothing`*** would be pointing to an invalid position in memory.

Rust will provide the following error if this code is typed out: ***`consider using the static lifetime`***. This is because our function is missing a lifetime identifier.

All functions that return a borrowed value must include a lifetime.

---

Slices let you reference a contiguous sequence of elements within a collection, instead of referencing and accessing the entire collection.

```rust
fn main()
{
	let mut s = String::from("Hello, world!");
	
	// Slices
	
	let hello = &s[0..5]; // Characters 0-4
	let world = &s[6..11]; // Characters 6-10
	
	let word = first_word(&s);
	
	s.clear();
}

fn first_word(
	s: &String
) -> usize {
	let bytes = s.as_bytes();
	
	for (i, &item) in bytes.iter().enumerate()
	{
		if item == b' '
		{
			return i;
		}
	}
	
	s.len()
}
```

A cool little, totally optional thing you can do when splicing strings i omit the 1st/last numbers if they refer to the beginning or the end of the string, like this:

```rust
fn main()
{
	...
	
	let hello = &s[..5]; // Characters 0-4
	let world = &s[6..]; // Characters 6-10
	
	...
}
```

Equally, if you omit *both*, it refers to the entire string.

```rust
fn main()
{
	...
	
	let word = &s[..];
	
	...
}
```

Now that we're aware of what slices are capable of, let's actually *fix* this function to properly do what we want.

```rust
fn main()
{
	let mut s = String::from("Hello, world!");
	let s2 = "Hello, world!"; // String literals are actually
							  //  string slices!
	
	let word = first_word(s2);
	
	...
}

fn first_word(
	s: &str
) -> &str {
	let bytes = s.as_bytes();
	
	for (i, &item) in bytes.iter().enumerate()
	{
		if item == b' '
		{
			return &s[0..i];
		}
	}
	
	&s[..]
}
```

One **last** thing. Slices work on regular arrays (shockingly) as well.

```rust
fn main()
{
	let a = [1, 2, 3, 4, 5];
	let slice = &a[..2]; // Type: &[i32]
}
```

Finally. We're done! That wasn't so bad, right?
