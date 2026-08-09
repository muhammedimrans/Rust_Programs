# Rust Mutability, Variables, Scope, and Integers

## 1. Immutability by Default 🛡️

By default, variables in Rust are **immutable**. This means that once a value is bound to a variable, it cannot be changed.

This design choice helps prevent unexpected side effects and makes code easier to reason about.

```rust
let x = 5; // x is immutable
// x = 6; // This would result in a compile-time error
```

---

## 2. Mutability with the `mut` Keyword 🔄

If you need to change the value of a variable, you must explicitly declare it as mutable using the `mut` keyword.

```rust
let mut y = 10; // y is mutable
y = 15;         // This is allowed

println!("The value of y is: {}", y);
// Output: The value of y is: 15
```

### Key idea

```rust
let x = 5;
```

means `x` cannot be reassigned.

```rust
let mut x = 5;
x = 10;
```

means `x` can be changed.

---

## 3. Type Inference and Type Annotations 💡

Rust is a **statically typed language**, meaning the type of every variable must be known at compile time.

However, you don't always need to explicitly write the type. Rust's **type inference** system can often determine the type based on the value you assign.

### Type inference

```rust
let a = 20;
```

Rust can infer that `a` is an integer. When no other information determines the integer type, Rust commonly defaults to `i32`.

### Explicit type annotation

```rust
let b: f64 = 3.14;
```

Here, `b` is explicitly declared as a 64-bit floating-point number.

The general syntax is:

```rust
let variable_name: type = value;
```

For example:

```rust
let age: u32 = 31;
let price: f64 = 99.99;
```

Even though Rust can often infer types, explicitly annotating types can improve code readability and clarity, especially for complex data structures or when the inferred type might not be immediately obvious.

---

## 4. Shadowing 🎭

Rust allows you to declare a new variable with the same name as a previous variable. This is called **shadowing**.

When you shadow a variable, the new variable takes precedence over the previous variable when you use that name.

Shadowing is different from making a variable mutable:

- **`mut`** changes the value of an existing variable binding.
- **Shadowing** creates a new variable with the same name.

For example:

```rust
let z = 25;          // First z
let z = z + 5;       // z is shadowed by a new z with value 30
let z = "hello";     // z is shadowed again and is now a string

println!("The value of z is: {}", z);
// Output: The value of z is: hello
```

One important advantage of shadowing is that the type can change:

```rust
let value = 10;
let value = value.to_string();
```

Here:

```text
value: i32
   ↓
shadowed
   ↓
value: String
```

With `mut`, changing the type of the same variable is not allowed:

```rust
let mut value = 10;
// value = "hello"; // ❌ Type mismatch
```

Shadowing is particularly useful when you want to transform a value while keeping the same variable name.

---

## 5. Scope 🌐

Variables in Rust have a specific **scope**, which is the region of code where they are valid.

Once a variable goes out of scope, Rust automatically drops it when appropriate. For values that own resources, this is part of Rust's ownership system and helps manage memory safely.

Example:

```rust
fn main() {
    let outer_var = 100; // outer_var is in scope here

    { // Start of a new inner scope
        let inner_var = 200; // inner_var is in scope here

        println!("Inner variable: {}", inner_var);
    } // inner_var goes out of scope here

    // println!("Inner variable: {}", inner_var);
    // This would be a compile-time error because inner_var is out of scope.

    println!("Outer variable: {}", outer_var);
}
```

### Scope visualization

```text
main scope
│
├── outer_var = 100
│
├── inner scope
│   └── inner_var = 200
│
└── inner_var is no longer accessible
```

The `outer_var` remains available because it was declared in the outer scope.

---

# Signed and Unsigned Integers

Rust provides several integer types.

There are two main categories:

### Signed integers

Signed integers can represent **positive numbers, negative numbers, and zero**.

They use the `i` prefix:

```text
i8
i16
i32
i64
i128
```

For example:

```rust
let temperature: i32 = -10;
let balance: i64 = 5000;
```

### Unsigned integers

Unsigned integers can represent **zero and positive numbers**, but not negative numbers.

They use the `u` prefix:

```text
u8
u16
u32
u64
u128
```

For example:

```rust
let age: u32 = 31;
let count: u64 = 1000;
```

---

## Integer Sizes

| Size | Signed | Unsigned |
|------|--------|----------|
| 8-bit | `i8` | `u8` |
| 16-bit | `i16` | `u16` |
| 32-bit | `i32` | `u32` |
| 64-bit | `i64` | `u64` |
| 128-bit | `i128` | `u128` |

Rust also provides:

```text
isize
usize
```

These are signed and unsigned integer types whose size depends on the target architecture.

For example:

```rust
let index: usize = 0;
```

`usize` is commonly used for array and vector indexes.

---

## Signed vs Unsigned Example

```rust
let positive: i32 = 10;
let negative: i32 = -10;

let count: u32 = 10;
// let invalid: u32 = -10; // ❌ Cannot store a negative value in u32
```

The important distinction is:

```text
i32 → can store negative and positive values
u32 → can store zero and positive values only
```

---

# Summary

Rust's variable system provides several useful safety features:

- **Immutable by default:** Variables cannot be changed unless `mut` is used.
- **`mut`:** Allows an existing variable binding to be reassigned.
- **Type inference:** Rust can often determine a variable's type automatically.
- **Type annotations:** Let you explicitly specify the type.
- **Shadowing:** Allows a new variable to reuse an existing variable's name, including changing its type.
- **Scope:** Controls where a variable can be accessed.
- **Signed integers:** Use `i8`, `i16`, `i32`, `i64`, and `i128` and can represent negative values.
- **Unsigned integers:** Use `u8`, `u16`, `u32`, `u64`, and `u128` and represent zero and positive values.

These characteristics contribute to Rust's reputation for **safety, performance, and reliable concurrency**.
