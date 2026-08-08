# DATA TYPES - 01

## 🔧 Rust Program Breakdown

```rust
#![allow(unused_variables)]
```
This attribute disables warnings about variables that are declared but not used.

```rust
fn main() {}
```
Entry point of the Rust program.

## 📜 String Printing and Escape Characters

```rust
println!("Dear Emily,\nHow have you been?");
```
Prints a multi-line string using `\n` (newline).

```rust
println!("\tOnce upon a time");
```
`\t` inserts a tab space at the beginning of the line.

```rust
println!("Juliet said \"I love you Romeo\"");
```
`\"` is used to include quotes inside a string.

```rust
let filepath: &str = "C:\\My Documents\\new\\videos";
```
`\\` is used to escape backslashes in file paths.

```rust
println!("{filepath}");
```
Incorrect formatting; should be `println!("{filepath}")` with a prefixed f style, or use `println!("{}", filepath);`.

## 🔢 Number Operations

```rust
let value: i32 = -15;
println!("value is {}", value.abs());
```
`abs()` returns the absolute value of value.

```rust
let white_space : &str = "      white comment     ";
println!("White space is {}", white_space.trim());
```
`trim()` removes leading/trailing spaces from a string.

```rust
println!("{}", value.pow(4));
```
Raises value to the power of 4 (value^4).

## 🔍 Float Operations

```rust
let pi: f64 = 3.141567634256473;
```
f64 type for floating-point numbers.

```rust
println!("pi floor value is {}", pi.floor());
```
`floor()` rounds down to nearest integer.

```rust
println!("pi ceil value is {}", pi.ceil());
```
`ceil()` rounds up to nearest integer.

```rust
println!("pi round value is {}", pi.round());
```
`round()` rounds to nearest integer.

```rust
println!("The pi value is {pi:.4}");
```
Format pi to 4 decimal places (Rust 1.58+ inline formatting).

```rust
println!("The pi value is {:.7}", pi);
```
Format pi to 7 decimal places, traditional format string style.

## 🔁 Type Casting

```rust
let miles_away: i32 = 18;
let miles_away_i8 = miles_away as i8;
```
Type cast from i32 to smaller type i8.

```rust
println!("{}", miles_away_i8);
```
Prints casted value.

## ➕➖ Basic Arithmetic

```rust
let addition: i32 = 5 + 4;
let subtraction: i32 = 10 - 6;
let multiplication: i32 = 3 * 4;
```
Basic math operations.

```rust
println!("Addition: {addition}, subtraction: {subtraction}, multiplication: {multiplication}");
```
Uses inline variable formatting (Rust 1.58+).

```rust
let floor_division: i32 = 5 / 3;
println!("{floor_division}");
```
Integer division (floor result).

```rust
let decimal_division: f64 = 5.0 / 3.0;
println!("{decimal_division}");
```
Float division.

```rust
let remainder: i32 = 9 % 2;
println!("{remainder}");
```
Modulus operator gives remainder.

## 🔎 Boolean Logic

```rust
let age: i32 = 18;
let is_young: bool = age < 15;
```
Boolean expression (age check).

```rust
println!("is young value is {}", is_young);
println!("{} {}", age.is_positive(), age.is_negative());
```
Boolean checks for sign of the integer.

```rust
println!("{}", true);
println!("{}", !true);
```
Logical NOT operation.

```rust
println!("{}", "coke" == "Coke");
println!("{}", "coke" != "Coke");
```
String comparisons, case-sensitive.

```rust
let purchased_ticket: bool = true;
let plane_on_ticket: bool = true;
let making_event: bool = purchased_ticket && plane_on_ticket;
```
Logical AND operation. Can also use OR (`||`) and NOT (`!`).

```rust
println!("It is {} that i will arrive as expected", making_event);
```
Conditional output.

## 🔤 Characters

```rust
let first_initial : char = 'B';
let second: char = '🥶';
```
Characters can include Unicode emojis.

```rust
println!("Printing First {} and Second {}", first_initial, second);

println!("{} and {}", first_initial.is_alphabetic(), second.is_alphabetic());
```
Check if character is alphabetic.

```rust
println!("{} and {}", first_initial.is_uppercase(), second.is_lowercase());
```
Check casing of characters.

## 🧮 Arrays

```rust
let numbers: [i32; 9] = [1,2,3,4,5,6,7,8,9];
```
Fixed-size array of integers.

```rust
let mut apples: [&str; 3] = ["Machintose", "Garunda", "Alphabine"];
apples[2] = "Autumn";
```
Mutable array of strings; index-based assignment.

```rust
let apple = apples[0];
println!("{}", apple);

println!("Numbers are {:?}", numbers);
```
`:?` debug format for arrays.

```rust
println!("apples are {:#?}", apples);
```
Pretty-print array.

```rust
println!("length of apples are {}", apples.len());
```
Get length of array.

```rust
dbg!(2 + 2);
```
`dbg!` prints debug info with file and line number.

## 🧩 Tuples

```rust
let employee: (&str, i32, bool, f32) = ("Sudarshan", 35, true, 25.42);
let (name, age, is_male, money) = employee;
```
Tuples hold different types. Values can be destructured.

```rust
println!("Name is {name} age is {age} and is_male {is_male}");
println!("{employee:#?}");
```
Tuple debug formatting.

## 📈 Ranges and Loops

```rust
let month_days: std::ops::Range<i8> = 1..31;
```
Range from 1 to 30 (31 not included).

```rust
println!("{:#?}", month_days);

for number in month_days {
    print!("{number}");
}
```
Loop over numbers using the range.

```rust
let letters = 'b'..'q';
for characters in letters {
    print!("{characters:# }");
}
```
Loop through characters from 'b' to 'p'
