# Ownership in Rust

## Topics Covered in this Section
- Ownership
- The Stack and the Heap
- The `Copy` Trait
- `str` vs the `String` Type
- The `push_str` Method
- Moves/Transfers of Ownership
- The `drop` Function
- The `clone` Method
- References and Borrows
- String Slices
- Functions and Ownership
- Mutable Parameters
- Returning Owned Values

---

### What is Ownership?

In Rust, every piece of data has an owner — like how every toy in your house belongs to one person. Rust uses this idea to manage memory safely — without needing a garbage collector (like in Python or Java).

- Simple types (numbers, bools, char) are copied.
- Heap types (like `String`, `Vec`, `Box`) are moved, unless you use `.clone()`.

#### Real-Life Example

Imagine you have a book 📕.
- You give the book to your friend.
- Now you don’t have it anymore.
- Only your friend can read or give it to someone else.
- This is like ownership in Rust.

#### Rust Code Example

```rust
fn main() {
    let name = String::from("Imran");
    let my_name = name;  // Ownership moved to `my_name`
    
    // println!("{}", name); // ❌ This will cause an error!
    println!("{}", my_name); // ✅ This works!
}
```

Explanation:
- `name` owns the string "Imran".
- When we do `let my_name = name;`, the ownership moves to `my_name`.
- Now `name` is no longer valid — you can’t use it again.

#### How to Fix This? Use `.clone()`

If you want two copies of the data:

```rust
fn main() {
    let name = String::from("Imran");
    let my_name = name.clone(); // Makes a copy

    println!("{}", name);      // ✅ Works now
    println!("{}", my_name);   // ✅ Works too
}
```

#### Special Case: Simple Values

With numbers (like `i32`), Rust copies by default — because they're small and cheap to copy.

```rust
fn main() {
    let a = 10;
    let b = a; // a is copied, not moved

    println!("a = {}, b = {}", a, b); // ✅ No problem!
}
```

Why Ownership Is Cool:
- It makes sure you never forget to free memory.
- It prevents bugs like double free or use-after-free.
- Rust programs are fast and safe because of ownership!

---

### Stack and Heap

**Stack**
- Like a plate stack in a cafeteria — you can only take the top plate.
- Fast and organized.
- Stores small, fixed-size things like numbers (`i32`, `bool`, `char`).

**Heap**
- Like a big messy storage room — you can put in and take out anything, but it’s slower.
- Stores big or growable stuff like `String`, `Vec`, etc.
- Needs more work to manage (Rust does this safely using ownership).

**Stack vs Heap: Simple Comparison**

| Feature | Stack | Heap |
|---|---:|---:|
| Speed | Fast | Slower |
| Size | Small, fixed-size | Big, flexible |
| Use case | Simple types (`i32`, `bool`) | Complex types (`String`, `Vec`) |
| Memory control | Automatic and simple | Rust uses ownership to manage |

#### Example in Rust

```rust
fn main() {
    let x = 10; // stored in the stack
    let name = String::from("Imran"); // name is in stack, but the string data is in heap

    println!("x = {}", x);
    println!("name = {}", name);
}
```

What Happens Behind the Scenes:
- `x = 10`:
  - Stored fully in the stack (very fast).
- `name = String::from("Imran")`:
  - A pointer, length, and capacity go to the stack.
  - The actual letters "Imran" go into the heap.

So the stack says:

name --> [pointer to heap | length: 5 | capacity: 5]

And the heap contains:

"Imran"

Why Does Rust Do This?
- Rust is like a smart teacher:
  - It wants you to clean up after yourself
  - So it remembers who owns what
  - It makes sure you don’t lose or mess up memory

That's why Rust is safe and fast!

---

### Summary You Can Remember

| Term | Like What? | Stores What? | Fast? |
|---|---|---:|---:|
| Stack | Neat pencil box | Small things (numbers) | ✅ Yes |
| Heap | Messy file cabinet | Big things (text, images, vectors) | ❌ Slower |
| Rust | Strict but smart teacher | Makes sure you don’t mess memory | ✅ Always |

---

### Program for Mutable reference

```rust
fn main() {
    // Create a new, empty String
    let mut current_meal: String = String::new();

    // Pass a mutable reference so we can modify the String
    add_flour(&mut current_meal);

    // Pass an immutable reference so we can only read/print
    show_my_meal(&current_meal);
}

// Function that takes a mutable reference (&mut String)
fn add_flour(meal: &mut String) {
    meal.push_str("Add Flour"); // ✅ corrected syntax
}

// Function that takes an immutable reference (&String)
fn show_my_meal(meal: &String) {
    println!("Meal Steps: {meal}");
}
```
