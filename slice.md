# Rust Slices — Simple Notes and Examples

## What is a Slice?

A **slice** is a view into a part of a collection.

A slice does **not own** the data. It only tells Rust:

> "Start here, and use these elements."

The two most common slices are:

- `&str` — a string slice
- `&[T]` — an array/vector slice

A useful way to remember it:

```text
Original data
+----+----+----+----+----+
| A  | B  | C  | D  | E  |
+----+----+----+----+----+
      ^--------------^
          slice
```

The original data is still there. The slice is only a view of part of it.

---

# 1. Introduction to Slice

A slice is a reference to a continuous part of a collection.

For example:

```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];

    let part = &numbers[1..4];

    println!("{:?}", part);
}
```

Output:

```text
[20, 30, 40]
```

Here:

```rust
&numbers[1..4]
```

means:

- Start at index `1`
- Stop before index `4`

So the selected indexes are:

```text
Index:   0   1   2   3   4
Value:  10  20  30  40  50
             |-----------|
             20  30  40
```

### Important

The end index is **exclusive**.

```rust
&numbers[1..4]
```

means:

```text
1, 2, 3
```

not:

```text
1, 2, 3, 4
```

## Different slice ranges

```rust
let numbers = [10, 20, 30, 40, 50];

let a = &numbers[..];   // Everything
let b = &numbers[..3];  // Index 0, 1, 2
let c = &numbers[2..];  // Index 2, 3, 4
let d = &numbers[1..4]; // Index 1, 2, 3
```

---

# 2. Creating a String Slice from a String

A `String` owns its data.

We can create a string slice (`&str`) from it:

```rust
fn main() {
    let message = String::from("Hello Rust");

    let part = &message[0..5];

    println!("{}", part);
}
```

Output:

```text
Hello
```

The `String` owns the text:

```text
message
   |
   v
"Hello Rust"
```

The slice only borrows part of it:

```text
message
   |
   +---- "Hello Rust"
             ^
             |
          &str slice
          "Hello"
```

The slice does not create a new `String`.

## Using a different range

```rust
fn main() {
    let message = String::from("Hello Rust");

    let rust = &message[6..10];

    println!("{}", rust);
}
```

Output:

```text
Rust
```

## Important: String slices use byte indexes

Rust strings are UTF-8.

For simple ASCII text:

```rust
let word = String::from("Hello");
let part = &word[0..2];

println!("{}", part);
```

This works because each ASCII character uses one byte.

But with characters such as:

```text
é
😊
न
```

one character can use multiple bytes.

Therefore, this can panic:

```rust
let word = String::from("😊");
let part = &word[0..1]; // Wrong boundary
```

The range must fall on a valid UTF-8 character boundary.

For beginners, remember:

> String slicing uses byte positions, not character positions.

---

# 3. String Slices and String Literals

A string literal such as:

```rust
let name = "Rust";
```

has the type:

```rust
&str
```

That means a string literal is already a string slice.

You can think of:

```rust
"Rust"
```

as a reference to string data stored in the program's binary.

Example:

```rust
fn main() {
    let name = "Rust";

    println!("{}", name);
}
```

Here:

```text
name
 |
 v
&str
 |
 v
"Rust"
```

## `String` vs `&str`

This difference is very important.

### String

```rust
let name = String::from("Rust");
```

`String`:

- Owns the text
- Stores data on the heap
- Can grow
- Is mutable when declared `mut`

Example:

```rust
let mut name = String::from("Rust");

name.push_str(" Programming");

println!("{}", name);
```

### String slice

```rust
let name: &str = "Rust";
```

`&str`:

- Does not own the text
- Borrows string data
- Is a view into string data
- Cannot be modified through an immutable `&str`

### Easy comparison

```text
String
    ↓
Owns the text

&str
    ↓
Borrows/views the text
```

---

# 4. String Slice Length

A string slice has a `.len()` method.

Important:

> `.len()` returns the number of **bytes**, not the number of characters.

Example:

```rust
fn main() {
    let word = "Hello";

    println!("{}", word.len());
}
```

Output:

```text
5
```

Because:

```text
H = 1 byte
e = 1 byte
l = 1 byte
l = 1 byte
o = 1 byte

Total = 5 bytes
```

But Unicode can be different:

```rust
fn main() {
    let word = "é";

    println!("{}", word.len());
}
```

`é` uses 2 bytes in UTF-8, so:

```text
word.len() = 2
```

If you want the number of Unicode scalar values:

```rust
let word = "é";

println!("{}", word.chars().count());
```

Output:

```text
1
```

So remember:

```rust
.len()
```

→ bytes

```rust
.chars().count()
```

→ Unicode scalar values

---

# 5. String Slice as a Function Parameter

One of the biggest reasons `&str` is useful is that functions can accept string slices.

Example:

```rust
fn print_name(name: &str) {
    println!("Hello, {}", name);
}

fn main() {
    let name = String::from("Imran");

    print_name(&name);
}
```

Output:

```text
Hello, Imran
```

Here:

```rust
print_name(&name);
```

creates a string slice/reference to the `String`.

## Why use `&str`?

Suppose we write:

```rust
fn print_name(name: String) {
    println!("{}", name);
}
```

The function takes ownership of the `String`.

But:

```rust
fn print_name(name: &str) {
    println!("{}", name);
}
```

only borrows the text.

This is more flexible.

It can accept a `String`:

```rust
let name = String::from("Imran");

print_name(&name);
```

And it can also accept a string literal:

```rust
print_name("Imran");
```

This is one reason `&str` is commonly used for function parameters when the function only needs to read text.

## Understanding example

```rust
fn first_word(text: &str) -> &str {
    for (i, byte) in text.bytes().enumerate() {
        if byte == b' ' {
            return &text[..i];
        }
    }

    text
}

fn main() {
    let sentence = String::from("Hello Rust");

    let word = first_word(&sentence);

    println!("{}", word);
}
```

Output:

```text
Hello
```

The function does not create a new `String`.

It returns a slice pointing into the original string.

---

# 6. Array Slice

Just like strings can be sliced, arrays can also be sliced.

Example:

```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];

    let slice = &numbers[1..4];

    println!("{:?}", slice);
}
```

Output:

```text
[20, 30, 40]
```

The type of `slice` is:

```rust
&[i32]
```

This means:

```text
&
reference

[i32]
slice containing i32 values
```

## Array and array slice are different

This:

```rust
let numbers = [10, 20, 30, 40, 50];
```

has type:

```rust
[i32; 5]
```

It is an array with exactly 5 elements.

This:

```rust
let slice = &numbers[1..4];
```

has type:

```rust
&[i32]
```

It is a reference to a slice.

### Easy way to remember

```text
[i32; 5]
    ↓
Array
    ↓
Fixed size: 5

&[i32]
    ↓
Slice reference
    ↓
Can represent different lengths
```

---

# 7. Deref Coercion of Array Slices

The term is **deref coercion**.

Rust can automatically convert certain references into another compatible reference type when calling a function.

For example:

```rust
fn print_numbers(numbers: &[i32]) {
    println!("{:?}", numbers);
}

fn main() {
    let numbers = [10, 20, 30, 40, 50];

    print_numbers(&numbers);
}
```

Here:

```rust
&numbers
```

is technically:

```rust
&[i32; 5]
```

But the function expects:

```rust
&[i32]
```

Rust automatically performs the needed **deref coercion**.

So this works:

```rust
print_numbers(&numbers);
```

You don't need to manually write:

```rust
print_numbers(&numbers[..]);
```

Both approaches can work:

```rust
print_numbers(&numbers);
print_numbers(&numbers[..]);
```

## Why is this useful?

A function accepting:

```rust
&[i32]
```

can work with arrays of different sizes.

```rust
fn print_numbers(numbers: &[i32]) {
    for number in numbers {
        println!("{}", number);
    }
}

fn main() {
    let a = [10, 20, 30];
    let b = [100, 200, 300, 400, 500];

    print_numbers(&a);
    print_numbers(&b);
}
```

The function doesn't care whether the array has 3 elements or 5 elements.

It only asks for:

```text
a slice of i32 values
```

This is one of the important benefits of slices.

---

# 8. Mutable Array Slice

A normal slice:

```rust
&[i32]
```

is an immutable view.

A mutable slice:

```rust
&mut [i32]
```

allows us to modify the elements.

Example:

```rust
fn double_numbers(numbers: &mut [i32]) {
    for number in numbers {
        *number *= 2;
    }
}

fn main() {
    let mut numbers = [1, 2, 3, 4, 5];

    double_numbers(&mut numbers);

    println!("{:?}", numbers);
}
```

Output:

```text
[2, 4, 6, 8, 10]
```

## Understanding `*number`

Inside:

```rust
for number in numbers {
    *number *= 2;
}
```

`number` is a mutable reference to an element.

For example:

```text
number
   |
   v
&mut i32
   |
   v
  10
```

The `*` operator dereferences the reference so we can access and modify the actual value:

```rust
*number *= 2;
```

## Modifying only part of an array

```rust
fn main() {
    let mut numbers = [10, 20, 30, 40, 50];

    let middle = &mut numbers[1..4];

    middle[0] = 200;
    middle[1] = 300;
    middle[2] = 400;

    println!("{:?}", numbers);
}
```

Output:

```text
[10, 200, 300, 400, 50]
```

Notice what happened:

```text
Original:

[10, 20, 30, 40, 50]
     ^^^^^^^^^^^^^^^
        mutable slice

After modification:

[10, 200, 300, 400, 50]
```

The slice changed the original array because the slice is a mutable reference to the original data.

---

# A Simple Real-World Example

Imagine you have marks for five students:

```rust
let mut marks = [50, 60, 70, 80, 90];
```

You want to add 5 marks only to the last three students.

```rust
fn add_bonus(marks: &mut [i32]) {
    for mark in marks {
        *mark += 5;
    }
}

fn main() {
    let mut marks = [50, 60, 70, 80, 90];

    add_bonus(&mut marks[2..]);

    println!("{:?}", marks);
}
```

Output:

```text
[50, 60, 75, 85, 95]
```

Only this portion was passed:

```rust
&mut marks[2..]
```

So the function could modify only:

```text
70
80
90
```

---

# Important Slice Concepts to Remember

## 1. A slice does not own the data

```rust
let numbers = [10, 20, 30];

let slice = &numbers[..];
```

`numbers` owns the array.

`slice` only borrows it.

---

## 2. Slice size is not fixed

An array:

```rust
[i32; 5]
```

always has 5 elements.

A slice:

```rust
&[i32]
```

can have:

```text
0 elements
1 element
2 elements
10 elements
100 elements
...
```

depending on what it references.

---

## 3. `&[T]` means immutable slice

```rust
let slice: &[i32] = &numbers[..];
```

You can read the elements but cannot modify them through this slice.

---

## 4. `&mut [T]` means mutable slice

```rust
let slice: &mut [i32] = &mut numbers[..];
```

You can modify the elements through the slice.

---

## 5. `&str` is a string slice

```rust
let name: &str = "Rust";
```

It is a borrowed view of UTF-8 text.

---

## 6. String slicing uses byte indexes

```rust
let text = String::from("Hello");

let slice = &text[0..2];
```

This gives:

```text
He
```

For Unicode text, indexes must be valid UTF-8 boundaries.

---

# Quick Comparison

| Type | Meaning | Owns Data? | Mutable? |
|---|---|---:|---:|
| `String` | Owned growable string | Yes | Yes, if `mut` |
| `&str` | String slice | No | No |
| `[i32; 5]` | Fixed-size array | Yes | Yes, if `mut` |
| `&[i32]` | Immutable array slice | No | No |
| `&mut [i32]` | Mutable array slice | No | Yes |

---

# Beginner Mental Model

Think of an array like a big chocolate bar:

```text
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
```

The array owns the whole chocolate bar.

A slice is like pointing at only part of it:

```text
+----+----+----+----+----+
| 10 | 20 | 30 | 40 | 50 |
+----+----+----+----+----+
      ^--------------^
           slice
```

The slice does not create another chocolate bar.

It simply says:

> "I want to look at this part."

A mutable slice says:

> "I want to change this part."

---

# Final Cheat Sheet

```rust
// String slice
let text = String::from("Hello Rust");
let slice = &text[0..5];

// String literal is &str
let name: &str = "Rust";

// Slice length
let length = slice.len();

// Function accepting string slice
fn print_text(text: &str) {
    println!("{}", text);
}

// Array
let numbers = [10, 20, 30, 40, 50];

// Immutable array slice
let slice = &numbers[1..4];

// Mutable array slice
let mut numbers = [10, 20, 30, 40, 50];
let slice = &mut numbers[1..4];

slice[0] = 200;
```

The most important idea is:

```text
Array/String
     |
     | borrow part of it
     v
   Slice
     |
     +---- &str       → string slice
     |
     +---- &[T]       → immutable slice
     |
     +---- &mut [T]   → mutable slice
```

> **Slice = a borrowed view into part of existing data.**
