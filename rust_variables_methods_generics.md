# Rust Variables, Constants, Methods, and Generics

These notes cover several important Rust concepts:

1. Unused variables and warnings
2. Immutability and `mut`
3. Rust compiler error codes
4. Constants
5. Type aliases
6. `#[allow(...)]` attributes
7. Strings and escape characters
8. Methods and `impl`
9. Associated functions
10. Instance methods and `self`
11. Generic types
12. Generic functions and trait bounds

---

# 1. Unused Variables and `_`

Rust warns you when you declare a variable but never use it.

For example:

```rust
fn main() {
    let number = 10;
}
```

Rust will normally produce a warning similar to:

```text
warning: unused variable: `number`
```

The warning comes from the `unused_variables` lint:

```text
#[warn(unused_variables)] on by default
```

## Using `_`

If you intentionally don't need a variable, you can prefix its name with `_`:

```rust
fn main() {
    let _number = 10;
}
```

This tells Rust:

> I intentionally don't plan to use this variable.

This prevents the unused-variable warning.

### Important

There is a difference between:

```rust
let _number = 10;
```

and:

```rust
let _ = 10;
```

`_number` is still a named variable that can be used later.

```rust
let _number = 10;
println!("{}", _number);
```

`_` is a special pattern meaning:

> Ignore this value completely.

```rust
let _ = 10;
```

---

# 2. Variables Are Immutable by Default

Rust variables are **immutable by default**.

```rust
let num = 2;
```

After this declaration, you cannot assign a new value to `num`:

```rust
let num = 2;

// num = 5; // ❌ Compile-time error
```

Think of immutable as:

> Once the variable is assigned, its binding cannot be reassigned.

This helps Rust prevent accidental changes to values.

---

# 3. Making a Variable Mutable with `mut`

If you want to change a variable, explicitly use `mut`:

```rust
let mut num = 2;

num = 5;

println!("{}", num);
```

Output:

```text
5
```

Compare:

```rust
let num = 2;
```

with:

```rust
let mut num = 2;
```

```text
let
 ↓
immutable binding

let mut
 ↓
mutable binding
```

`mut` gives permission to reassign the variable.

---

# 4. Compiler Error Codes

Rust compiler errors often have unique error codes.

For example, if you try to modify an immutable variable:

```rust
fn main() {
    let num = 2;
    num = 5;
}
```

Rust may report an error such as:

```text
error[E0384]: cannot assign twice to immutable variable `num`
```

Rust provides documentation for error codes.

You can use:

```bash
rustc --explain E0384
```

This displays an explanation of `E0384`.

If the explanation opens in a terminal pager, press:

```text
q
```

to quit.

### General pattern

When Rust gives you:

```text
error[E0384]
```

you can often investigate it with:

```bash
rustc --explain E0384
```

This is a very useful Rust debugging habit.

---

# 5. Constants with `const`

Rust allows you to declare constants using `const`.

Example:

```rust
const TAX_RATE: f32 = 234.32;
```

The syntax is:

```rust
const NAME: TYPE = VALUE;
```

For example:

```rust
const MAX_USERS: u32 = 1000;
const PI_VALUE: f64 = 3.14159;
```

Constants are different from ordinary variables.

```rust
let x = 10;
```

creates a variable binding.

```rust
const MAX_USERS: u32 = 1000;
```

creates a constant.

### Important

Constants must have an explicit type:

```rust
const TAX_RATE: f32 = 234.32;
```

Unlike ordinary variables, you cannot rely on type inference for the constant declaration.

Constants are also immutable and cannot be declared with `mut`.

This is invalid:

```rust
// const mut VALUE: i32 = 10; // ❌
```

---

# 6. Type Aliases

A **type alias** gives another name to an existing type.

Syntax:

```rust
type Meters = i32;
```

Now `Meters` is another name for `i32`.

Example:

```rust
type Meters = i32;

fn main() {
    let distance: Meters = 1600;

    println!("Distance: {} meters", distance);
}
```

Here:

```text
Meters
   ↓
 i32
```

The compiler still treats `Meters` as `i32`.

### Why use type aliases?

They can make code easier to understand.

Instead of:

```rust
let distance: i32 = 1600;
```

you can write:

```rust
let distance: Meters = 1600;
```

The second version communicates what the number represents.

---

# 7. `#[allow(unused_variables)]`

Rust attributes can control compiler lints.

For example:

```rust
#[allow(unused_variables)]
fn main() {
    let number = 10;
}
```

This tells Rust:

> Don't warn about unused variables inside this function.

You can also put the attribute on another item where the lint should be allowed.

## Allowing it for the entire program

If you want to suppress the warning throughout the entire crate, use:

```rust
#![allow(unused_variables)]
```

Notice the `!`.

```text
#[allow(...)]
 ↑
applies to the item it is attached to

#![allow(...)]
  ↑
applies at the crate/program level
```

Example:

```rust
#![allow(unused_variables)]

fn main() {
    let number = 10;
    let another_number = 20;
}
```

Both unused-variable warnings are suppressed.

### Suppressing all warnings

You can also write:

```rust
#![allow(warnings)]
```

However, this is generally not recommended for production code because warnings can identify useful problems.

It is usually better to fix the warning or intentionally prefix an unused variable with `_`.

---

# 8. Strings and Escape Characters

Rust strings use double quotes:

```rust
let name = "Emily";
```

You can put special characters into strings using escape sequences.

## New line: `\n`

```rust
println!("Dear Emily,\nHow have you been?");
```

Output:

```text
Dear Emily,
How have you been?
```

`\n` means:

> Start a new line.

---

## Tab: `\t`

```rust
println!("\tOnce upon a time");
```

`\t` inserts a tab.

---

## Double quote: `\"`

If you want quotation marks inside a string, escape them:

```rust
println!("Juliet said \"I love you Romeo\"");
```

Output:

```text
Juliet said "I love you Romeo"
```

Without `\"`, the inner quotation marks would terminate the string.

---

# 9. Backslashes in File Paths

A Windows path such as:

```text
C:\My Documents\new\videos
```

contains backslashes.

In a normal Rust string, backslash starts an escape sequence, so you need to escape the backslashes:

```rust
let filepath: &str = "C:\\My Documents\\new\\videos";

println!("{filepath}");
```

Output:

```text
C:\My Documents\new\videos
```

## Raw strings

Rust also provides raw strings, which can be convenient for paths:

```rust
let filepath = r"C:\My Documents\new\videos";

println!("{filepath}");
```

The `r` before the string means Rust should treat the contents more literally.

---

# 10. Rust Methods — Actions for Your Data

A useful way to understand methods is to imagine a toy car.

### Data

The toy car itself has information:

```text
color
number of wheels
speed
```

In Rust, this kind of data can be represented using a `struct`.

### Methods

The toy car can perform actions:

```text
drive()
stop()
change_color()
```

Methods are functions associated with a type.

Rust has two important forms:

1. **Associated functions**
2. **Instance methods**

---

# 11. Associated Functions

An associated function belongs to the type itself rather than to a particular instance.

You call an associated function using:

```text
TypeName::function_name()
```

Notice the `::`.

Example:

```rust
struct Dog {
    name: String,
    breed: String,
}

impl Dog {
    fn new(name: String, breed: String) -> Dog {
        Dog { name, breed }
    }

    fn how_many_legs() -> u8 {
        4
    }
}

fn main() {
    let my_dog = Dog::new(
        String::from("Buddy"),
        String::from("Golden Retriever"),
    );

    println!("My dog's name: {}", my_dog.name);

    let legs = Dog::how_many_legs();

    println!("Dogs usually have {} legs.", legs);
}
```

Output:

```text
My dog's name: Buddy
Dogs usually have 4 legs.
```

Notice:

```rust
Dog::new(...)
```

and:

```rust
Dog::how_many_legs()
```

The function is called using the **type name**.

---

# 12. Why `::`?

The double colon:

```rust
::
```

is commonly used to access an associated item of a type or module.

For example:

```rust
Dog::new(...)
```

means:

> Call the `new` associated function belonging to `Dog`.

Another common example:

```rust
String::from("Hello")
```

Here `from` is an associated function of `String`.

---

# 13. Instance Methods

An instance method operates on a specific value.

You call it using:

```text
variable.method_name()
```

For example:

```rust
my_phone.check_battery()
```

The dot:

```text
.
```

is used because we're working with a particular instance.

---

# 14. `self`, `&self`, and `&mut self`

Rust uses `self` to represent the current instance.

There are three important forms.

## `&self`

```rust
fn check_battery(&self) -> u8
```

Means:

> Borrow the current object so I can read it, but I won't modify it.

Think:

```text
&self
 ↓
read-only access
```

---

## `&mut self`

```rust
fn charge(&mut self, amount: u8)
```

Means:

> Borrow the current object mutably because I want to modify it.

Think:

```text
&mut self
 ↓
read + modify
```

The object itself must be mutable when calling such a method.

---

## `self`

```rust
fn consume(self)
```

Means:

> Take ownership of the current object.

After the method consumes `self`, the original value generally cannot be used again.

This is less common for ordinary modifying methods but is useful when a method is intended to consume the object.

---

# 15. Instance Method Example

```rust
struct Phone {
    brand: String,
    battery_percentage: u8,
}

impl Phone {
    // Reads the phone without changing it.
    fn check_battery(&self) -> u8 {
        self.battery_percentage
    }

    // Changes the phone.
    fn charge(&mut self, amount: u8) {
        self.battery_percentage += amount;

        if self.battery_percentage > 100 {
            self.battery_percentage = 100;
        }

        println!(
            "Phone charged! Now at {}%",
            self.battery_percentage
        );
    }
}

fn main() {
    let mut my_phone = Phone {
        brand: String::from("AwesomePhone"),
        battery_percentage: 50,
    };

    println!(
        "My phone's battery is: {}%",
        my_phone.check_battery()
    );

    my_phone.charge(30);

    println!(
        "My phone's battery is now: {}%",
        my_phone.check_battery()
    );
}
```

Output:

```text
My phone's battery is: 50%
Phone charged! Now at 80%
My phone's battery is now: 80%
```

---

# 16. Why `my_phone` Needs `mut`

Notice:

```rust
let mut my_phone = Phone {
```

We need `mut` because:

```rust
my_phone.charge(30);
```

calls a method that takes:

```rust
&mut self
```

The method changes:

```rust
battery_percentage
```

Therefore the instance must be mutable.

---

# 17. `impl` Block

The `impl` keyword is used to define functionality associated with a type.

Example:

```rust
impl Phone {
    fn check_battery(&self) -> u8 {
        self.battery_percentage
    }
}
```

Think of:

```text
struct
  ↓
defines the data

impl
  ↓
defines what the data can do
```

For example:

```text
Phone
├── brand
├── battery_percentage
│
└── methods
    ├── check_battery()
    └── charge()
```

---

# 18. Easy Method Cheat Sheet

| Syntax | Meaning |
|---|---|
| `Type::function()` | Associated function |
| `variable.method()` | Instance method |
| `&self` | Read the instance |
| `&mut self` | Modify the instance |
| `self` | Take ownership of the instance |
| `impl Type` | Define functionality for a type |

Remember:

```rust
Dog::new()
```

uses:

```text
::
```

because it is an associated function.

While:

```rust
my_dog.check_name()
```

uses:

```text
.
```

because it operates on a specific instance.

---

# 19. Generic Types

Generics allow you to write code that works with **different types** without duplicating the same code.

Think of a universal lunchbox.

A normal lunchbox might only hold one specific type of food.

A generic lunchbox can hold different types of food.

In Rust, a generic type acts like a placeholder.

Common generic names include:

```text
T = Type
E = Element/Error
K = Key
V = Value
```

---

# 20. Generic Struct

Suppose we want a box that can hold an integer:

```rust
struct IntegerBox {
    value: i32,
}
```

And another box for a string:

```rust
struct StringBox {
    value: String,
}
```

If we continue doing this for every type, the code becomes repetitive.

Generics solve this.

```rust
struct GenericBox<T> {
    value: T,
}
```

Here:

```text
T
↓
placeholder for a type
```

The compiler determines what `T` represents when the struct is used.

---

# 21. Using a Generic Struct

```rust
struct GenericBox<T> {
    value: T,
}

fn main() {
    let integer_box = GenericBox {
        value: 100,
    };

    let string_box = GenericBox {
        value: String::from("Hello, Rust!"),
    };

    let float_box: GenericBox<f64> = GenericBox {
        value: 3.14,
    };

    println!("Integer box value: {}", integer_box.value);
    println!("String box value: {}", string_box.value);
    println!("Float box value: {}", float_box.value);
}
```

Output:

```text
Integer box value: 100
String box value: Hello, Rust!
Float box value: 3.14
```

The same struct works with:

```text
i32
String
f64
```

---

# 22. What Does `<T>` Mean?

When you write:

```rust
struct GenericBox<T>
```

you're saying:

> GenericBox has a type parameter called `T`.

Then:

```rust
struct GenericBox<T> {
    value: T,
}
```

means:

> The `value` field has whatever type `T` represents.

For example:

```rust
GenericBox<i32>
```

means:

```text
T = i32
```

while:

```rust
GenericBox<String>
```

means:

```text
T = String
```

---

# 23. Generic Functions

Generics can also be used with functions.

Suppose we have a function that finds the largest integer:

```rust
fn largest_i32(list: &[i32]) -> i32 {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

We would need another function for `f64`, another for another type, and so on.

Generics let us write one reusable function.

---

# 24. Generic `largest` Function

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}
```

This function can work with different types, provided those types satisfy the required traits.

---

# 25. Understanding `T: PartialOrd + Copy`

This is called a **trait bound**.

```rust
T: PartialOrd + Copy
```

means:

> `T` can be any type, but it must implement `PartialOrd` and `Copy`.

### `PartialOrd`

The function needs to compare values:

```rust
if item > largest
```

So `T` needs to support ordering/comparison.

### `Copy`

The function uses:

```rust
for &item in list
```

and needs to copy the value out for the local variable.

Therefore `T` needs to implement `Copy`.

---

# 26. Using the Generic Function

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result_int = largest(&number_list);

    println!("The largest number is {}", result_int);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result_char = largest(&char_list);

    println!("The largest char is {}", result_char);
}
```

Output:

```text
The largest number is 100
The largest char is y
```

The same `largest` function works for both:

```text
Vec<i32>
```

and:

```text
Vec<char>
```

because both types satisfy the required trait bounds.

---

# 27. Why Generics Are Useful

Without generics:

```text
largest_i32()
largest_f64()
largest_u32()
largest_i64()
...
```

You may end up repeating the same logic.

With generics:

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T
```

you write the logic once.

### Main benefits

#### Code reusability

Write one piece of code that works with multiple types.

#### Type safety

Rust still checks the types at compile time.

Generics do not mean:

> Anything can be put anywhere.

The compiler still makes sure the types satisfy the required rules.

#### Trait bounds

Trait bounds allow you to specify what a generic type must be able to do.

For example:

```rust
T: PartialOrd
```

means `T` must support the required ordering operations.

---

# 28. Generics and Performance

Rust generally uses **monomorphization** for generics.

Conceptually, when you use:

```rust
largest(&number_list);
```

and:

```rust
largest(&char_list);
```

the compiler generates specialized code for the concrete types.

This allows Rust to provide generic, reusable source code without requiring the same kind of runtime dispatch overhead associated with some other generic systems.

In simple terms:

```text
Generic source code
       ↓
Compile time
       ↓
Specialized machine code
       ↓
Fast execution
```

---

# Quick Revision

## Variables

```rust
let x = 10;
```

Immutable.

```rust
let mut x = 10;
x = 20;
```

Mutable.

---

## Ignore an unused variable

```rust
let _value = 10;
```

---

## Constants

```rust
const TAX_RATE: f32 = 234.32;
```

---

## Type aliases

```rust
type Meters = i32;

let distance: Meters = 1600;
```

---

## Allow warnings

For one item:

```rust
#[allow(unused_variables)]
```

For the entire crate:

```rust
#![allow(unused_variables)]
```

---

## Compiler error documentation

```bash
rustc --explain E0384
```

---

## Associated function

```rust
Dog::new()
```

Uses `::`.

---

## Instance method

```rust
my_dog.check_name()
```

Uses `.`.

---

## Method receiver

```rust
&self
```

Read.

```rust
&mut self
```

Modify.

```rust
self
```

Take ownership.

---

## Generic struct

```rust
struct Box<T> {
    value: T,
}
```

`T` is a type placeholder.

---

## Generic function

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    // ...
}
```

`T` can represent different types as long as the required trait bounds are satisfied.

---

# Big Picture

These concepts are connected to Rust's core philosophy:

```text
Immutable by default
        ↓
Explicit mutation with mut
        ↓
Strong compile-time type checking
        ↓
Ownership and scope
        ↓
Traits and generics
        ↓
Reusable + safe + performant code
```

Rust makes many decisions explicit rather than silently allowing potentially unsafe or unintended behavior.
