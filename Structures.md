Structs allow you to group related data together. Think about them as object attributes in an object oriented language.

```rust
struct User {
	username: String,
	email: String,
	signed_in_count: u64,
	active: bool
}

fn main()
{
	let mut user1 = User {
		email: String::from("myemail@domain.com"),
		username: String::from("transicle"),
		active: true,
		signed_in_count: 1
	};
	
	let name = user1.username;
	user1.username = String::from("my_new_name");
}
```

To change data in a structure instance, you have to make the entirety of the structure mutable, rather than a single field.

We can also use functions to construct new instances of ***`User`***.

```rust
...

fn build_user(
	email: String,
	username: String
) -> User {
	User {
		email: email,
		username: username,
		active: true,
		signed_in_count: 1
	}
}
```

A really interesting part about creating structures, is that, if the variable you've created/are assigning into the structure is the same name as the field you're trying to assign to, then you can omit the field itself and just leave the value.

This is called: **The Field Init Shorthand Syntax**

```rust
...

fn build_user(
	email: String,
	username: String
) -> User {
	User {
		email,
		username,
		active: true,
		signed_in_count: 1
	}
}
```

```rust
...

fn main()
{
	let mut user1 = User {
		email: String::from("myemail@domain.com"),
		username: String::from("transicle"),
		active: true,
		signed_in_count: 1
	};
	
	let name = user1.username;
	user1.username = String::from("my_new_name");
	
	let user2 = build_user(
		String::from("myemail2@domain.com"),
		String::from("my_new_name2")
	);
}
```

Another nice, convenient feature is that we can create new instances of a structure using existing instances.

```rust
...

fn main()
{
	let mut user1 = User {
		email: String::from("myemail@domain.com"),
		username: String::from("transicle"),
		active: true,
		signed_in_count: 1
	};
	
	let name = user1.username;
	user1.username = String::from("my_new_name");
	
	let user2 = build_user(
		String::from("myemail2@domain.com"),
		String::from("my_new_name2")
	);
	
	let user3 = User {
		email: String::from("myemail3@domain.com"),
		username: String::from("my_new_name3"),
		..user2
	} // Copies the `active` and `signed_in_count` fields
	  //  from `user2`.
}
```

---

We can also create structs without name fields, these are called tuple structs.

```rust
fn main()
{
	struct Color(i32, i32, i32);
	struct Point(i32, i32, i32);
}
```

Tuple structs are useful when you want your entire tuple to have a name and be of different type than other tuples. So, as an example, ***`Color`*** and ***`Point`*** both have the same field types, which are 3 signed 32-bit integers, but they are of different type.

---

Implementation blocks will house the functions and methods associated with our structure.

```rust
struct Rectangle {
	width: u32,
	height: u32
}

impl Rectangle
{
	fn area(
		&self
	) -> u32 {
		self.width * self.height
	}
}

// Can now call `.area` on a Rectangle instance.
```
