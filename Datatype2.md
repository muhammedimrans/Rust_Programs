# DATA TYPES - 02

Useful Data Actions (Methods) in Rust

A. Text Data (String and &str)

String is owned, growable text. &str is a borrowed, fixed-size view into text. Many methods work on both, or &str methods produce a String.

Common methods

- `.trim()` — Removes whitespace (spaces, tabs, newlines) from the start and end.
- `.to_uppercase()` / `.to_lowercase()` — Convert all letters to UPPERCASE or lowercase.
- `.contains("other_text")` — Returns true if the text includes the given substring.
- `.starts_with("prefix")` / `.ends_with("suffix")` — Check prefix/suffix.
- `.replace("old", "new")` — Replace all occurrences of `old` with `new` and return a new String.
- `.len()` — Returns length in bytes. (For character count use `.chars().count()`.)
- `.is_empty()` — Returns true if the text has no characters.
- `.split("separator")` — Split into an iterator of substrings by the separator.

Quick details and examples

- `.trim()`
  - What it does: Removes whitespace from start and end.
  - Useful for: Cleaning user input.
  - Example: `"  hello  ".trim()` -> `"hello"`

- `.to_uppercase()` / `.to_lowercase()`
  - What they do: Convert case of letters.
  - Useful for: Case-insensitive comparisons.
  - Example: `"Hello".to_uppercase()` -> `"HELLO"`

- `.contains("other_text")`
  - What it does: Checks for a substring.
  - Example: `"apple pie".contains("pie")` -> `true`

- `.starts_with("prefix")` / `.ends_with("suffix")`
  - What they do: Check beginning/end of text.
  - Example: `"document.pdf".ends_with(".pdf")` -> `true`

- `.replace("old", "new")`
  - What it does: Replaces all occurrences of `old` with `new`.
  - Example: `"cat dog cat".replace("cat", "bird")` -> `"bird dog bird"`

- `.len()`
  - What it does: Length in bytes. For ASCII this equals characters; for Unicode use `.chars().count()` for character count.
  - Example: `"hello".len()` -> `5`

- `.is_empty()`
  - Example: `"".is_empty()` -> `true`

- `.split("separator")`
  - What it does: Splits into substrings.
  - Example: `"a,b,c".split(',').collect::<Vec<&str>>()` -> `vec!["a","b","c"]`

---

B. Integer Data (i32, u64, etc. — whole numbers)

These methods operate on integer types (signed and unsigned).

- `.abs()` — Absolute value (signed integers only).
  - Example: `(-10i32).abs()` -> `10`

- `.pow(exp)` — Raise to an integer power. The argument is `u32`.
  - Example: `(2i32).pow(3)` -> `8`

- `.is_positive()` / `.is_negative()` — Sign checks (signed integers only).
  - Example: `(-5i32).is_negative()` -> `true`

- `.count_ones()` / `.count_zeros()` — Count 1-bits or 0-bits in the binary representation (type-specific width).
  - Example: `(5u8).count_ones()` -> `2` (5 is 00000101)

---

C. Float Data (f32, f64 — numbers with decimals)

Floats have methods for numeric operations and special-value checks.

- `.abs()` — Absolute value.
  - Example: `(-3.14f64).abs()` -> `3.14`

- `.sqrt()` — Square root.
  - Example: `(9.0f64).sqrt()` -> `3.0`

- `.floor()` / `.ceil()` / `.round()`
  - `floor()` — round down
  - `ceil()` — round up
  - `round()` — round to nearest
  - Example: `(3.7f64).floor()` -> `3.0`, `(3.2f64).ceil()` -> `4.0`, `(3.7f64).round()` -> `4.0`

- `.is_nan()` — Check for Not-a-Number (NaN).
  - Example: `(0.0f64 / 0.0).is_nan()` -> `true`

- `.is_infinite()` / `.is_normal()` — Check for infinity or normal finite value.
  - Example: `(1.0f64 / 0.0).is_infinite()` -> `true`

---

D. Boolean Data (bool — true/false)

Booleans are simple but have a couple of handy helpers:

- `.then_some(value)`
  - What it does: If the boolean is true, returns `Some(value)`, otherwise `None`.
  - Example:

```rust
let condition = true;
let maybe_number = condition.then_some(123); // Some(123)
let other_condition = false;
let maybe_text = other_condition.then_some("hello"); // None
```

- `.then(|| expr)`
  - What it does: If true, runs the closure and returns `Some(result)`, otherwise `None`.
  - Example:

```rust
let debug_mode = true;
let message = debug_mode.then(|| String::from("Debug info generated!"));
// message is Some("Debug info generated!")

let debug_mode_off = false;
let no_message = debug_mode_off.then(|| String::from("No debug info."));
// no_message is None
```

---

The Golden Rule

To discover all methods for a type, consult the official Rust documentation: https://doc.rust-lang.org/std/
Search for the type name (e.g., `String`, `i32`, `f64`, `bool`) and read the "Methods" section.

---

Program to demonstrate these data-type methods

```rust
fn main() {
    // Text examples
    let s: &str = "  Hello, Rust!  ";
    println!("Trimmed: '{}'", s.trim());
    println!("Uppercase: {}", s.to_uppercase());
    println!("Contains 'Rust': {}", s.contains("Rust"));

    // Integer examples
    let n: i32 = -10;
    println!("abs: {}", n.abs());
    println!("pow: {}", (2i32).pow(3));
    println!("is_negative: {}", n.is_negative());

    // Float examples
    let x: f64 = 3.7;
    println!("floor: {} ceil: {} round: {}", x.floor(), x.ceil(), x.round());
    println!("sqrt of 9: {}", (9.0f64).sqrt());

    // Bool examples
    let ok = true;
    let maybe = ok.then_some("proceed");
    println!("maybe: {:?}", maybe);
}
```

---

## Detailed String Methods Examples

### 1. trim()

Removes whitespace from the beginning and end of a string.

```rust
fn main() {
    let text = "   Hello Rust   ";

    let result = text.trim();

    println!("Before: '{text}'");
    println!("After: '{result}'");
}
```

Output:

```
Before: '   Hello Rust   '
After: 'Hello Rust'
```

**Important:** `trim()` doesn't modify the original String. It returns a trimmed view (`&str`).

### 2. .to_uppercase()

Converts letters to uppercase.

```rust
fn main() {
    let text = "hello rust";

    let result = text.to_uppercase();

    println!("{result}");
}
```

Output:

```
HELLO RUST
```

### 3. .to_lowercase()

Converts letters to lowercase.

```rust
fn main() {
    let text = "HELLO RUST";

    let result = text.to_lowercase();

    println!("{result}");
}
```

Output:

```
hello rust
```

You can combine them:

```rust
fn main() {
    let text = "Hello Rust";

    println!("{}", text.to_uppercase());
    println!("{}", text.to_lowercase());
}
```

### 4. .contains()

Checks whether a string contains another string.

```rust
fn main() {
    let text = "I am learning Rust";

    let result = text.contains("Rust");

    println!("Contains Rust? {result}");
}
```

Output:

```
Contains Rust? true
```

If you search for something that doesn't exist:

```rust
let result = text.contains("Python");
```

Output:

```
false
```

**The important thing is:**

`contains()` returns a boolean: `true` or `false`

### 5. .starts_with()

Checks whether the string starts with specific text.

```rust
fn main() {
    let text = "Hello Rust";

    let result = text.starts_with("Hello");

    println!("Starts with Hello? {result}");
}
```

Output:

```
Starts with Hello? true
```

For example:

```rust
text.starts_with("Rust")
```

would return:

```
false
```

### 6. .ends_with()

Checks whether the string ends with specific text.

```rust
fn main() {
    let text = "Hello Rust";

    let result = text.ends_with("Rust");

    println!("Ends with Rust? {result}");
}
```

Output:

```
Ends with Rust? true
```

### 7. .replace()

Replaces text with other text.

```rust
fn main() {
    let text = "I like Python";

    let result = text.replace("Python", "Rust");

    println!("{result}");
}
```

Output:

```
I like Rust
```

Notice that the original text is unchanged:

```rust
fn main() {
    let text = "I like Python";

    let result = text.replace("Python", "Rust");

    println!("Original: {text}");
    println!("New: {result}");
}
```

Output:

```
Original: I like Python
New: I like Rust
```

So: `replace()` creates a new String

### 8. .len()

Returns the length in bytes.

```rust
fn main() {
    let text = "Hello";

    let length = text.len();

    println!("Length: {length}");
}
```

Output:

```
Length: 5
```

Each character in "Hello" is one byte, so:

```
H e l l o
1 1 1 1 1 = 5 bytes
```

**Important with Unicode**

```rust
fn main() {
    let text = "你好";

    println!("Bytes: {}", text.len());
    println!("Characters: {}", text.chars().count());
}
```

The output will be:

```
Bytes: 6
Characters: 2
```

So remember:

```
text.len()           // bytes
text.chars().count() // characters
```

### 9. .is_empty()

Checks whether a string contains nothing.

```rust
fn main() {
    let text = "";

    let result = text.is_empty();

    println!("Is empty? {result}");
}
```

Output:

```
Is empty? true
```

With text:

```rust
fn main() {
    let text = "Hello";

    println!("Is empty? {}", text.is_empty());
}
```

Output:

```
Is empty? false
```

This is commonly useful in if statements:

```rust
fn main() {
    let text = "";

    if text.is_empty() {
        println!("No text was provided");
    } else {
        println!("Text was provided");
    }
}
```

### 10. .split()

This one is slightly different.

`split()` breaks a string into multiple pieces.

For example:

```rust
fn main() {
    let text = "apple,banana,orange";

    for fruit in text.split(",") {
        println!("{fruit}");
    }
}
```

Output:

```
apple
banana
orange
```

Here's what's happening:

```
"apple,banana,orange"
       ↓ split(",")
       
"apple"   "banana"   "orange"
```

`split()` returns an iterator, which is why we commonly use it with a for loop.

You can also collect the pieces:

```rust
fn main() {
    let text = "apple,banana,orange";

    let fruits: Vec<&str> = text.split(",").collect();

    println!("{fruits:?}");
}
```

Output:

```
["apple", "banana", "orange"]
```

### Quick cheat sheet

| Method | What it does | Returns |
|--------|------------|---------|
| trim() | Removes beginning/end whitespace | &str |
| to_uppercase() | Converts to uppercase | String |
| to_lowercase() | Converts to lowercase | String |
| contains() | Checks if text exists | bool |
| starts_with() | Checks beginning | bool |
| ends_with() | Checks ending | bool |
| replace() | Replaces text | String |
| len() | Gets byte length | usize |
| is_empty() | Checks if empty | bool |
| split() | Splits text | iterator |

### One thing I recommend remembering

The methods fall into a few groups:

**Modify/produce text:**
- trim()
- to_uppercase()
- to_lowercase()
- replace()

**Ask a question about text:**
- contains()
- starts_with()
- ends_with()
- is_empty()

These return `true`/`false`.

**Get information from text:**
- len()
- chars().count()

**Break text apart:**
- split()

Once you understand these groups, Rust's string methods become much easier to remember.
