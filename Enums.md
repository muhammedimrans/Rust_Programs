# 🟢 Enums in Rust

## What is an Enum?

An **enum** is a way to create a custom type by listing its possible variants. Enums help group related options under a single name, making your code expressive and type-safe.

---

## 🚦 The Analogy: A Traffic Light

Imagine a traffic light. At any given moment, it can only be in one of three states:
1. Red
2. Yellow
3. Green

It can't be red and green at the same time—these are the only allowed options.

An enum (short for "enumeration") in Rust is just like this: a type with a fixed set of possible values, and a variable of that type can only be one of those values at a time.

---

## 📝 Simple Example: A TrafficLight Enum

### 1. Define the Enum

You define an enum using the `enum` keyword, followed by the name and curly braces with the possible variants.

```rust
// This defines our custom TrafficLight type
enum TrafficLight {
    Red,
    Yellow,
    Green,
}
```

Now, `TrafficLight` is a new data type in your program, and its only possible values are `TrafficLight::Red`, `TrafficLight::Yellow`, and `TrafficLight::Green`.

---

### 2. Create a Variable of the Enum Type

You create an instance of the enum by specifying the enum's name, followed by `::` and the variant.

```rust
fn main() {
    let current_light = TrafficLight::Green;

    // You can't assign it to anything else
    // current_light = "blue"; // This would cause a compile error!
}
```

---

### 3. Use a Match Expression to Handle the Variants

The most powerful way to work with enums is with a `match` expression. It allows you to execute different code based on which variant the enum variable holds. It's exhaustive, meaning you must handle every possible variant.

```rust
fn main() {
    let current_light = TrafficLight::Yellow;

    match current_light {
        TrafficLight::Red => {
            println!("Stop! The light is red.");
        }
        TrafficLight::Yellow => {
            println!("Slow down! The light is yellow.");
        }
        TrafficLight::Green => {
            println!("Go! The light is green.");
        }
    }
}
```
**Output:**
```
Slow down! The light is yellow.
```

---

## 💪 Enums with Associated Data (The Superpower!)

Enums can also hold data with each variant—this is where they become incredibly powerful.

**Analogy:** A `Message` enum could have variants like `Quit`, `Move`, `Write`, and `ChangeColor`:
- `Quit`: No extra data.
- `Move`: Needs coordinates (x, y).
- `Write`: Needs a string of text.
- `ChangeColor`: Needs RGB values.

---

### Example: A Message Enum

```rust
// This enum has variants with different types of data attached.
enum Message {
    Quit,                         // No data
    Move { x: i32, y: i32 },      // An anonymous struct
    Write(String),                // A single String
    ChangeColor(i32, i32, i32),   // A tuple of three i32s
}

fn process_message(msg: Message) {
    match msg {
        Message::Quit => {
            println!("Received Quit message. Shutting down.");
        }
        Message::Move { x, y } => {
            println!("Received Move message. Moving to x: {}, y: {}", x, y);
        }
        Message::Write(text) => {
            println!("Received Write message. Text: '{}'", text);
        }
        Message::ChangeColor(r, g, b) => {
            println!("Received ChangeColor message. New color is R: {}, G: {}, B: {}", r, g, b);
        }
    }
}

fn main() {
    let msg1 = Message::Move { x: 10, y: 5 };
    let msg2 = Message::Write(String::from("Hello, Rust!"));
    let msg3 = Message::ChangeColor(255, 0, 0);
    let msg4 = Message::Quit;

    process_message(msg1);
    process_message(msg2);
    process_message(msg3);
    process_message(msg4);
}
```

**Output:**
```
Received Move message. Moving to x: 10, y: 5
Received Write message. Text: 'Hello, Rust!'
Received ChangeColor message. New color is R: 255, G: 0, B: 0
Received Quit message. Shutting down.
```

---

## 🗝️ Key Takeaways

- **Enumeration:** An enum defines a set of named variants.
- **One at a time:** A variable can only be one variant at any given moment.
- **Type Safety:** Prevents you from using an invalid value (like `"blue"` for a traffic light).
- **Associated Data:** Each variant can optionally hold its own, different type of data.
- **`match`:** The primary way to handle an enum's variants and extract its data.

---

Paste this directly into your README or documentation page for a clear, beginner-friendly explanation of Rust enums!
