# Rust_Programs
Certainly! Here’s your Rust program explanation, reformatted as a README.md file with proper Markdown headings and code blocks for clarity.

---

# Rust Data Types & Syntax Walkthrough

This document explains each line of a simple Rust program, demonstrating core Rust syntax, types, and operations.

---

## 🔧 Program Entry and Attributes

```rust
#![allow(unused_variables)]
fn main() {
    // ... your code here ...
}
```
- `#![allow(unused_variables)]`: Tells the Rust compiler to suppress warnings about unused variables.
- `fn main()`: The entry point of every Rust program.

---

## 📜 String Printing & Escape Characters

```rust
println!("Dear Emily,\nHow have you been?");
println!("\tOnce upon a time");
println!("Juliet said \"I love you Romeo\"");
let filepath: &str = "C:\\My Documents\\new\\videos";
println!("{filepath}");
```
- `\n` adds a newline.
- `\t` adds a tab space.
- `\"` allows quotes inside strings.
- Double backslashes (`\\`) escape backslashes in file paths.
- `{filepath}` uses Rust 1.58+ inline formatting.

---

## 🔢 Numbers and Math

```rust
let value: i32 = -15;
println!("value is {}", value.abs());
println!("{}", value.pow(4));
```
- `.abs()` returns the absolute value.
- `.pow(4)` raises the number to the 4th power.

---

## ✂️ Whitespace Handling

```rust
let white_space: &str = "      white comment     ";
println!("White space is {}", white_space.trim());
```
- `.trim()` removes leading and trailing whitespace.

---

## 🔍 Floating Point Operations

```rust
let pi: f64 = 3.141567634256473;
println!("pi floor value is {}", pi.floor());
println!("pi ceil value is {}", pi.ceil());
println!("pi round value is {}", pi.round());
println!("The pi value is {pi:.4}");
println!("The pi value is {:.7}", pi);
```
- `.floor()`: Largest integer ≤ value
- `.ceil()`: Smallest integer ≥ value
- `.round()`: Nearest integer
- Formatting with `{pi:.4}` gives 4 decimal places, `{:.7}` gives 7 decimal places.

---

## 🔁 Type Casting

```rust
let miles_away: i32 = 18;
let miles_away_i8 = miles_away as i8;
println!("{}", miles_away_i8);
```
- `as` keyword is used to cast types (e.g., i32 to i8).

---

## ➕➖ Arithmetic Operators

```rust
let addition = 5 + 4;
let subtraction = 10 - 6;
let multiplication = 3 * 4;
let floor_division = 5 / 3;
let decimal_division = 5.0 / 3.0;
let remainder = 9 % 2;
```
- `/` is integer division if both operands are integers.
- `%` is modulus (remainder).
- Floating point division requires at least one float operand.

---

## 🔎 Boolean Logic

```rust
let age = 18;
let is_young = age < 15;
println!("{}", is_young);
println!("{}", age.is_positive());
println!("{}", age.is_negative());

println!("{}", true);
println!("{}", !true);
println!("{}", "coke" == "Coke");
println!("{}", "coke" != "Coke");

let purchased_ticket = true;
let plane_on_ticket = true;
let making_event = purchased_ticket && plane_on_ticket;
```
- Basic comparisons return a `bool`.
- `.is_positive()` and `.is_negative()` check a number's sign.
- `&&` is logical AND, `||` is logical OR, `!` is logical NOT.
- String comparison is case-sensitive.

---

## 🔤 Characters

```rust
let first_initial: char = 'B';
let second: char = '🥶';
println!("{}", first_initial.is_alphabetic());
println!("{}", first_initial.is_uppercase());
```
- `char` can store Unicode values, including emojis.
- Character property checks: alphabetic, uppercase, etc.

---

## 📚 Arrays

```rust
let numbers = [1,2,3,4,5,6,7,8,9];
let mut apples = ["Machintose", "Garunda", "Alphabine"];
apples[2] = "Autumn";
println!("{:?}", numbers);
println!("{:#?}", apples);
println!("{}", apples.len());
dbg!(2 + 2);
```
- Arrays are fixed-size, homogenous collections.
- Arrays can be mutable (`mut`) and indexed.
- `:?` and `#?` are used for debug-style printing.
- `dbg!()` is for quick debugging with extra context.

---

## 🧩 Tuples

```rust
let employee = ("Sudarshan", 35, true, 25.42);
let (name, age, is_male, money) = employee;
println!("Name is {name} age is {age} and is_male {is_male}");
println!("{employee:#?}");
```
- Tuples store values of different types.
- Access by index or destructuring.

---

## 🔁 Ranges & Loops

```rust
let month_days = 1..31;
for number in month_days {
    print!("{number}");
}

let letters = 'b'..'q';
for characters in letters {
    print!("{characters:# }");
}
```
- `1..31`: Range from 1 to 30 (exclusive).
- Loop through the range using `for`.
- Loop through a range of characters (`'b'` to `'p'`).

---

Here’s your content reformatted as a README.md-style page with clear headings, code blocks, and explanations—ready to paste into your repo!

---

# 📄 DATA TYPES 02

## Useful Data Actions (Methods) in Rust

This guide covers common data types in Rust and their most useful methods, with simple explanations and examples.

---

## A. Text Data (`String` and `&str`)

- **`String`**: Text you own and can change/grow.
- **`&str`**: Borrowed, fixed-size view of text.
- Many methods work on both, or `&str` methods produce a `String`.

### `.trim()`
Removes blank spaces (spaces, tabs, newlines) from the start and end.

- **Useful for:** Cleaning up user input.
- **Example:**
    ```rust
    "  hello  ".trim() // "hello"
    ```

### `.to_uppercase()` / `.to_lowercase()`
Changes all letters to UPPERCASE or lowercase.

- **Useful for:** Making text consistent for comparisons.
- **Example:**
    ```rust
    "Hello".to_uppercase() // "HELLO"
    ```

### `.contains("other_text")`
Checks if your text includes a smaller piece of text.

- **Useful for:** Searching for words.
- **Example:**
    ```rust
    "apple pie".contains("pie") // true
    ```

### `.starts_with("prefix")` / `.ends_with("suffix")`
Checks if your text begins or ends with specific text.

- **Useful for:** Checking file extensions, URL types.
- **Example:**
    ```rust
    "document.pdf".ends_with(".pdf") // true
    ```

### `.replace("old", "new")`
Finds all occurrences of "old" text and swaps with "new".

- **Useful for:** Correcting words, formatting.
- **Example:**
    ```rust
    "cat dog cat".replace("cat", "bird") // "bird dog bird"
    ```

### `.len()`
Gives the length of the text in bytes (usually the number of characters in simple English).

- **Useful for:** Basic length checks.
- **Example:**
    ```rust
    "hello".len() // 5
    ```

### `.is_empty()`
Checks if the text has no characters at all.

- **Useful for:** Making sure input isn't blank.
- **Example:**
    ```rust
    "".is_empty() // true
    ```

### `.split("separator")`
Breaks text into pieces wherever it sees the "separator".

- **Useful for:** Splitting sentences, processing CSV data.
- **Example:**
    ```rust
    "a,b,c".split(",") // "a", "b", "c"
    ```

---

## B. Integer Data (`i32`, `u64`, etc.)

Whole numbers. Methods usually operate directly on the number.

### `.abs()`
Gives the "absolute value" (removes minus sign).

- **Useful for:** Getting size regardless of sign.
- **Example:**
    ```rust
    (-10).abs() // 10
    (5).abs()   // 5
    ```

### `.pow(exponent)`
Raises the number to a power.

- **Useful for:** Math calculations.
- **Example:**
    ```rust
    (2).pow(3) // 8
    ```

### `.is_positive()` / `.is_negative()`
Checks if the number is greater than or less than zero (signed only).

- **Useful for:** Conditional logic.
- **Example:**
    ```rust
    (-5).is_negative() // true
    ```

### `.count_ones()` / `.count_zeros()`
Counts number of 1s or 0s in the number's binary form.

- **Useful for:** Bit manipulation, algorithms.
- **Example:**
    ```rust
    (5u8).count_ones() // 2  (5 in binary: 00000101)
    ```

---

## C. Float Data (`f32`, `f64`)

Numbers with decimals. Methods handle decimals and special values.

### `.abs()`
Same as integers—gives absolute value.

- **Example:**
    ```rust
    (-3.14).abs() // 3.14
    ```

### `.sqrt()`
Calculates the square root.

- **Useful for:** Geometry, physics.
- **Example:**
    ```rust
    (9.0).sqrt() // 3.0
    ```

### `.floor()` / `.ceil()` / `.round()`
- `.floor()`: Rounds down to nearest whole number.
- `.ceil()`: Rounds up to nearest whole number.
- `.round()`: Standard rounding to nearest whole number.

- **Example:**
    ```rust
    (3.7).floor() // 3.0
    (3.2).ceil()  // 4.0
    (3.7).round() // 4.0
    (3.2).round() // 3.0
    ```

### `.is_nan()`
Checks if the number is "Not a Number" (NaN).

- **Useful for:** Error checking in calculations.
- **Example:**
    ```rust
    (0.0 / 0.0).is_nan() // true
    ```

### `.is_infinite()` / `.is_normal()`
Checks for special floating-point values.

- **Useful for:** Handling extreme results.
- **Example:**
    ```rust
    (1.0 / 0.0).is_infinite() // true
    ```

---

## D. Boolean Data (`bool`)

Booleans are simple but powerful!

### `.then_some(value)`
If true, returns `Some(value)`. If false, returns `None`.

- **Useful for:** Creating Option values based on a condition, without an if statement.
- **Example:**
    ```rust
    let condition = true;
    let maybe_number = condition.then_some(123); // Some(123)
    let other_condition = false;
    let maybe_text = other_condition.then_some("hello"); // None
    ```

### `.then(f)`
If true, runs function `f` and wraps result in `Some()`. If false, returns `None`.

- **Useful for:** Running code only if a condition is met.
- **Example:**
    ```rust
    let debug_mode = true;
    let message = debug_mode.then(|| String::from("Debug info generated!"));
    // message: Some("Debug info generated!")

    let debug_mode_off = false;
    let no_message = debug_mode_off.then(|| String::from("No debug info."));
    // no_message: None
    ```

---

## 📚 Remember the Golden Rule

For any data type in Rust, to see what methods it can perform, visit the [official Rust documentation](https://doc.rust-lang.org/std/) and search for the type name (e.g., `String`, `i32`, `f64`, `bool`). The "Methods" section lists everything!

---


Copy and paste this into your `README.md` file for a well-structured and informative documentation! If you want additional formatting or sections, let me know!
