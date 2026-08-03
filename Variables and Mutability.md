By default, all variables are immutable, or constant, they do not change. To modify a variable after declaration, using the ***`mut`*** keyword is required.

```rust
let x = 1; // Type: u32, Mutable?: No
let mut y = 2; // Type: u32, Mutable?: Yes

x = 2; // Would cause compile-time error.
y = 3; // Would properly re-assign.
```

Constants are similar to immutable variables, they are intended for permanent immutability, they should be used for values you know are always going to remain constant (i.e., the speed of light, how much starter health a player has, etc.)

Unlike variables, however, they cannot infer types. The type of the constant must be explicitly stated before compile time.

```rust
const SPEED_OF_LIGHT: u32 = 299792458; // m/s
```

The Rust formatting standard also suggests you write comments with this ***`UPPER_SNAKE_CASE`*** style. One last thing to note about constants, is they have to be able to be computed at compile-time, so if you have a constant be the output of a user input, that would error, because that *has* to be computed at runtime.

---

Immutabile variables doesn't mean we can't ever re-define that variable, it just means we cannot change the data that variable holds.

Shadowing a variable means to rebind a specific variable name to a different point, essentially re-defining it.

```rust
let x = 1; // Type: u32, Mutable?: No
let x = 2; // Type: u32, Mutable?: No
```

This is completely valid and acceptable code, however it's not the intended purpose of shadowing. A better use case would be to update a variable using itself, like this.

```rust
let x = 1; // Type: u32, Mutable?: No
let x = x + 1; // Type: u32, Mutable?: No
```

Now this value points to itself + 1, which depends on the version before that. The purpose of shadowing in itself is often times to shift the type of a variable.

```rust
let spaces: &str = "    "; // Type: str&, Mutable?: No
let spaces: u32 = spaces.len(); // Type: u32, Mutable?: No

println!("There are {} spaces.", spaces);
```
