An enum is a type that can be one of several named variant, and each variant can optionally carry data. Enums allow us to enumerate over a list of variants.

Let's look at this example:

```rust
enum IpAddrKind {
	v4,
	v6
}

fn main() {
	let four = IpAddrKind::v4;
	let six = IpAddrKind::v6;
}
```

Variants are namespaced under their identifier, so in this case, we use ***`::`*** to specify each of our variants (v4, v6). Our two variables, 4 and 6, are both variants of the ***`IpAddrKind`*** type. This means, we could for example, define a function that takes in this type and we could provide either an IPv4 or v6.

```rust
fn route(
	ip_kind: IpAddrKind
) {
	...
}
```

Our enum alone is able to capture the *version* of the IP address, but what if we want data associated with it too?

```rust
enum IpAddrKind {
	v4,
	v6
}

struct IpAddr {
	kind: IpAddrKind,
	address: String
}

fn main() {
	let four = IpAddrKind::v4;
	let six = IpAddrKind::v6;
	
	let localhost = IpAddr {
		kind: IpAddrKind::v4,
		address: String::from("127.0.0.1")
	}
}
```

The ***`IpAddr`*** struct allows us to group the version of the IP address alongside the address itself. But we can make this *even* more concise by putting data directly inside of the enum variant.

To store data inside the variants, we can add parenthesis after the variant and specify what type of data we want to store.

```rust
enum IpAddrKind {
	v4(String),
	v6(String)
}
```

And then further optimize the code above by rewriting the ***`localhost`*** variable.

```rust
enum IpAddrKind {
	v4(String),
	v6(String)
}

struct IpAddr {
	kind: IpAddrKind,
	address: String
}

fn main() {
	let localhost = IpAddrKind::v4(String::from("127.0.0.1"));
}
```

A single variant can carry multiple values of any types, mixed freely.

```rust
enum IpAddrKind {
	v4(u8, u8, u8, u8),
	v6(String)
}

struct IpAddr {
	kind: IpAddrKind,
	address: String
}

fn main() {
	let localhost = IpAddrKind::v4(127, 0, 0, 1);
}
```

> A single variant can carry multiple values of any types, mixed freely.

To show this properly, here's an example ***`Message`*** enum that holds 4 variants with 4 completely separate values/types.

```rust
enum Message {
	Quit,
	Move { // Anonymous Struct
		x: i32,
		y: i32
	},
	
	Write(String),
	ChangeColor(i32, i32, i32)
}
```

And just like struts, enums can also have methods and associated functions, and we can define them similarly to how we do for structures, with the ***`impl`*** keyword.

```rust
impl Message {
	fn some_function() {
		println!("Associated function in message!");
	}
}
```

---

Next, let's talk about the ***`Option`*** enum. Many languages have null values, and null values represent a useful concept: A value could either exist, or null (non-existent).

The problem with null values is that the type system in the language can't guarantee that if you use a value, it's not null. However, in Rust, null values do not exist. Instead, we have the ***`Option`*** enum.

```rust
fn main() {
	enum Option<T> {
		Some(T),
		None
	}
}
```

You have two variants in the enum presented, ***`Some`*** and ***`None`***, where ***`Some`*** can store any one value, of any type, due to it being a generic.

Or ***`None`***, which stores no value. If you have a value that *could* potentially be none, or may not exist, then you would wrap it in the ***`Option`*** enum.

```rust
// Example of Optionals

fn main() {
	let some_number = Some(5); // Type: Option<i32>
	let some_string = Some("a string");
	
	let absent_number = None; // This itself would fail compilation,
							  //  as there is no value.
							  
	// For it to work, we need to explicitly state its type.
	
	// Corrected:
	
	let absent_number: Option<i32> = None;
}
```

```rust
fn main() {
	let x = 5;
	let y = Some(5); // Type: Option<i8>
	
	let sum = x + y; // Compile-time error due to trying to add
					 //  two variables that do not share a type.
					 
	// To actually add them together, you need some way to extract
	//  the value outside of the variable.
	
	// That is where `.unwrap_or` comes in.
}
```

To actually add them together, you need some way to extract the value outside of the variable. That is where ***`.unwrap_or`*** comes in.

***`.unwrap_or`*** is a method that uses the value, in our case 5 if it exists, however, if it doesn't, then it uses the default value provided to it.

```rust
fn main() {
	let x = 5;
	let y = Some(5);
	
	let sum = x + y.unwrap_or(0); // If y is None, add against 0
}
```

---

Match expressions allows you to compare a value against a set of patterns. These patterns can be literals, variables, wild cars, and many other types.

The match expression is *exhaustive*, meaning we have to create a case for all possible cases. This makes the match expression very useful for things like enums.

For example, in the following case, we have a coin enum, which enumerates various coin types. And then we have a function that takes in a coin, then runs a match expression, then finally, for every type of coin, we simply return its value.

```rust
fn main() {

}

enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter
}

fn value_in_cents(
	coin: Coin
) -> u8 {
	match coin {
		Coin::Penny => 1,
		Coin::Nickel => 5,
		Coin::Dime => 10,
		Coin::Quarter => 25
	}
}
```

Patterns in a match expression can also bind to values. To show this, let's create another enum, which represents the state minted on each quarter.

```rust
fn main() {
	value_in_cents(Coin::Quarter(UsState::Alaska)); // Works!
}

#[derive(Debugu)]
enum UsState {
	Alabama,
	Alaska,
	Arizona,
	Arkansas,
	California,
	
	// ...
}

enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter(UsState)
}

fn value_in_cents(
	coin: Coin
) -> u8 {
	match coin {
		Coin::Penny => {
			println!("Lucky penny!");
			1
		}
		
		Coin::Nickel => 5,
		Coin::Dime => 10,
		Coin::Quarter(
			state
		) => {
			println!("State quarter is from {:?}", state);
			25
		},
	}
}
```

You can combine the match expression alongside the Option enum. 

```rust
fn main() {
	let five = Some(5);
	let six = plus_one(five);
	let none = plus_one(None);
}

fn plus_one(
	x: Option<i32>
) -> Option<i32> {
	match x {
		None => None,
		Some(i) => Some(i + 1), // `i` refers to the value.
								// We have to wrap it because our return
								//  value is an Optional.
	}
}
```

Since match expressions are exhaustive, we have to support *every* scenario, which can be.. exhausting. To deal with this, Rust allows us to handle all cases that are not explicitly handled like this:

```rust
match x {
	Some(i) => Some(i + 1),
	_ => ... // Do whatever we want here
}
```

---

The last thing that's going to be talked about is the ***`if let`*** syntax.

```rust
fn main() {
	let some_value = Some(3);
	
	match some_value {
		Some(3) => println!("3!"),
		_ => ()
	}
}
```

In this case, we only care about one scenario, a Some case with the value of 3, so it's a little overboard. Let's rewrite this using the if-let syntax.

```rust
fn main() {
	let some_value = Some(3);
	
	// If `some_value` matches `Some(3)`, print "3!".
	// If-let statements are typically read backwards.
	
	if let Some(3) = some_value {
		println!("3!");
	}
}
```
