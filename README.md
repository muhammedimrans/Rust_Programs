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

## ✅ Summary

This program is a practical walkthrough of:

- Basic I/O
- Variables & Types
- Arithmetic & Logic
- Strings, Characters
- Arrays & Tuples
- Ranges & Loops
- Formatting, Debugging, and Type Casting

---

Copy and paste this into your `README.md` file for a well-structured and informative documentation! If you want additional formatting or sections, let me know!
