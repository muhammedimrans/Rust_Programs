# Structs in Rust

## What is a Struct?

A struct (short for "structure") is a way to create a custom data type by grouping related data together. It allows you to give a name to a collection of different fields, making your code more organized and readable.

Instead of having three separate variables like `x`, `y`, and `z` for a 3D point, you can group them all into one `Point3D` struct.

---

## Simple Struct Example: A User Profile

Suppose you want to store information about a user on a website: their name, age, and whether they are active.

**Without a struct:**
```rust
let user_name = String::from("Alice");
let user_age = 30;
let user_is_active = true;
```
These three pieces of data are related, but they're just floating around as separate variables.

**With a struct:**

### Step 1: Define the struct
```rust
struct User {
    username: String,
    age: u32,       // u32 is a positive integer type
    is_active: bool,
}
```

### Step 2: Create an instance of the struct
```rust
fn main() {
    let user1 = User {
        username: String::from("Alice"),
        age: 30,
        is_active: true,
    };
}
```

### Step 3: Access data from the struct
```rust
fn main() {
    let user1 = User {
        username: String::from("Alice"),
        age: 30,
        is_active: true,
    };

    println!("User name: {}", user1.username);
    println!("User age: {}", user1.age);
    println!("Is active: {}", user1.is_active);

    let next_year_age = user1.age + 1;
    println!("Next year's age: {}", next_year_age);
}
```

### Step 4: Make a struct mutable

By default, struct instances are immutable. To change the values of the fields, you need to make the instance mutable:
```rust
fn main() {
    let mut user2 = User {
        username: String::from("Bob"),
        age: 25,
        is_active: false,
    };

    user2.is_active = true;
    user2.age = 26;

    println!("User name: {}", user2.username);
    println!("Is active now: {}", user2.is_active);
}
```

---

## Types of Structs

### 1. Tuple Structs

These are like regular tuples, but with a name. Useful when you want to group data without naming each field.

```rust
struct Point3D(i32, i32, i32);

fn main() {
    let origin = Point3D(0, 0, 0);
    println!("X coordinate: {}", origin.0);
    println!("Y coordinate: {}", origin.1);
    println!("Z coordinate: {}", origin.2);
}
```

### 2. Unit Structs

Structs with no fields. Useful for trait implementations on types that don't hold data.

```rust
struct AlwaysEmpty;

fn main() {
    let subject = AlwaysEmpty;
    // No fields to access!
}
```

---

## Why Use Structs?

- **Organization:** Keeps related data together.
- **Readability:** Makes code easier to understand.
- **Reusability:** Create many instances from the same blueprint.
- **Behavior (Methods):** You can define functions (methods) associated with your struct.

---

## Struct Mutation and Ownership Patterns

Imagine you have a valuable book (struct instance). Consider how functions (librarians) can access or mutate this "book":

### 1. Immutable struct value (`self`)

You give your book to the librarian forever—they can read but not change it, and you can't use it anymore.

```rust
struct User {
    name: String,
    age: u32,
}

impl User {
    fn introduce_self(self) {
        println!("Hello, my name is {} and I am {} years old.", self.name, self.age);
    }
}

fn main() {
    let user1 = User { name: String::from("Alice"), age: 30 };
    user1.introduce_self();
    // user1 is now unusable
}
```
*When to use*: Rarely; mainly for "consuming" methods.

### 2. Mutable struct value (`mut self`)

You give the book away forever, but allow the librarian to write in it.

```rust
impl User {
    fn birthday_and_consume(mut self) {
        self.age += 1;
        println!("Happy birthday! You are now {} years old.", self.age);
    }
}
```
*When to use*: Very rare.

### 3. Immutable reference (`&self`)

You let the librarian read your book, but you keep it.

```rust
impl User {
    fn describe_self(&self) {
        println!("This user is named {} and is {} years old.", self.name, self.age);
    }
}
```
*When to use*: Almost always when only reading data.

### 4. Mutable reference (`&mut self`)

You lend the book to the librarian, allowing edits, but you keep ownership.

```rust
impl User {
    fn celebrate_birthday(&mut self) {
        self.age += 1;
        println!("Happy birthday! You are now {} years old.", self.age);
    }
}
```
*When to use*: When you need to modify the struct in-place.

---

## What is `#[derive(Debug)]`?

### The Analogy: Giving Your Struct a "Print-Friendly Recipe"

By default, you can't print a struct directly in Rust. `#[derive(Debug)]` is an annotation that automatically implements the `Debug` trait for your struct, allowing you to print it for debugging:

```rust
#[derive(Debug)]
struct User {
    name: String,
    age: u32,
}

fn main() {
    let user1 = User { name: String::from("Alice"), age: 30 };
    println!("The user is: {:?}", user1);
    println!("The user is:\n{:#?}", user1);
}
```
- `{:?}`: Compact debug print.
- `{:#?}`: Pretty-printed debug print.

---

## Calling One Method from Another Within the Same Struct

### Analogy: A Car's Dashboard

A method can call other methods of the same struct via `self` (or `&self`, `&mut self`).

```rust
#[derive(Debug)]
struct Car {
    is_engine_on: bool,
    fuel_level: f64,
}

impl Car {
    fn new(fuel: f64) -> Car {
        Car { is_engine_on: false, fuel_level: fuel }
    }

    fn check_fuel(&self) {
        if self.fuel_level > 5.0 {
            println!("✅ Fuel level is good! ({} liters)", self.fuel_level);
        } else {
            println!("❌ Warning: Fuel is low! ({} liters)", self.fuel_level);
        }
    }

    fn turn_on_engine(&mut self) {
        if self.is_engine_on {
            println!("Engine is already on.");
        } else {
            println!("Starting the engine...");
            self.is_engine_on = true;
        }
    }

    fn prepare_for_drive(&mut self) {
        println!("\n--- Preparing the car for a drive ---");
        self.check_fuel();
        self.turn_on_engine();
        println!("--- Preparation complete. Ready to go! ---");
    }
}

fn main() {
    let mut my_car = Car::new(20.0);
    my_car.prepare_for_drive();
}
```
#### Key Points:
1. Use `self.method_name()` to call other methods.
2. Mutability must match: If you call a mutable method, the calling method must also be mutable.
3. This pattern supports clear, encapsulated logic.

---

## Associated Functions

### Analogy: A Car Factory's Assembly Line

- **Methods**: Actions on an instance, e.g., `my_car.start_engine()`.
- **Associated Functions**: Functions related to the type itself, not an instance (don't take `self`). Most commonly used as constructors (like `new`).

```rust
#[derive(Debug)]
struct User {
    username: String,
    age: u32,
    is_active: bool,
}

impl User {
    // Associated function (constructor)
    fn new(username: String, age: u32) -> User {
        User {
            username,
            age,
            is_active: true,
        }
    }
}

fn main() {
    let user1 = User::new(String::from("Alice"), 25);
    println!("{:#?}", user1);
}
```
You call an associated function on the type itself: `User::new(...)`.

---

## Summary

- **Structs** organize related data into custom types.
- **Mutability and ownership** patterns control how structs are changed or passed to functions.
- Use `#[derive(Debug)]` for debug printing.
- Methods can call each other via `self`.
- **Associated functions** (like `new`) are used as constructors and don't need an instance to be called.

Rust structs are a powerful way to model real-world data in a type-safe, readable manner!
