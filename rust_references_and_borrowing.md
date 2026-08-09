# Rust References and Borrowing

## The Big Idea

Rust lets you use data without taking ownership of it. This is called **borrowing**.

Think of a school notebook:

- Your friend owns the notebook.
- You borrow it to read.
- You give it back.
- Your friend still owns it.

In Rust:

```text
Owner
  ↓
owns the data

Reference
  ↓
borrows the data temporarily
```

The Rust Book describes a reference as a way to access data owned by another variable without taking ownership. fileciteturn0file0L3-L5

---

# 1. Why Do We Need References?

Consider this example:

```rust
fn main() {
    let name = String::from("Alice");

    print_name(name);

    println!("Name is: {name}");
}

fn print_name(name: String) {
    println!("Name: {name}");
}
```

This causes an ownership problem because the `String` is moved into `print_name`.

The function receives ownership of the String, so the original `name` cannot be used afterward.

The Rust Book explains this as the String being moved into the function. fileciteturn0file0L3-L5

---

# 2. Borrowing with `&`

Instead of giving the function ownership, give it a **reference**:

```rust
fn main() {
    let name = String::from("Alice");

    print_name(&name);

    println!("Name is: {name}");
}

fn print_name(name: &String) {
    println!("Name: {name}");
}
```

Output:

```text
Name: Alice
Name is: Alice
```

The important parts are:

```rust
print_name(&name);
```

and:

```rust
fn print_name(name: &String)
```

The `&` means:

> "Borrow this value. Don't take ownership."

This is the purpose of references described in the source. fileciteturn0file0L18-L23

---

# 3. Think of `&` Like Borrowing a Book

Imagine:

```text
You own a book.
       ↓
     BOOK
       ↓
Your friend borrows it.
       ↓
Friend reads it.
       ↓
Friend gives it back.
       ↓
You still own the book.
```

Rust works similarly:

```rust
let book = String::from("Rust");

read_book(&book);
```

The function gets access to the book, but it does not become the owner.

---

# 4. What Does `&String` Mean?

Look at:

```rust
fn print_name(name: &String) {
    println!("{name}");
}
```

Read:

```rust
name: &String
```

as:

> `name` is a reference to a `String`.

It does not own the String.

It can access the String owned by someone else.

The source explains that `&s1` creates a reference and `&String` tells the function parameter to expect a reference. fileciteturn0file0L23-L23

---

# 5. Borrowing Does Not Destroy the Original Value

```rust
fn main() {
    let name = String::from("Alice");

    print_name(&name);

    println!("I still have: {name}");
}

fn print_name(name: &String) {
    println!("The name is: {name}");
}
```

Output:

```text
The name is: Alice
I still have: Alice
```

Why?

Because the function **borrowed** the String instead of taking ownership.

---

# 6. Simple Function Example

Let's calculate the length of a String:

```rust
fn main() {
    let message = String::from("Hello");

    let length = calculate_length(&message);

    println!("Message: {message}");
    println!("Length: {length}");
}

fn calculate_length(text: &String) -> usize {
    text.len()
}
```

Output:

```text
Message: Hello
Length: 5
```

The function only needs to read the String, so it borrows it.

The source gives this same core pattern using `&s1` and `&String`. fileciteturn0file0L10-L23

---

# 7. What Happens When the Function Ends?

```rust
fn print_name(name: &String) {
    println!("{name}");
}
```

Here, `name` is only a reference.

When the function ends:

```text
name reference
     ↓
goes away

original String
     ↓
still exists
```

The String is not dropped because the reference did not own it. fileciteturn0file0L45-L51

---

# 8. What Is Borrowing?

The act of creating a reference is called **borrowing**.

```rust
let name = String::from("Alice");

print_name(&name);
```

Here:

```rust
&name
```

means:

> Borrow `name`.

The source describes borrowing using the same real-world idea: you can borrow something from its owner without becoming the owner. fileciteturn0file0L51-L53

---

# 9. Immutable References

References are read-only by default.

This does not work:

```rust
fn main() {
    let name = String::from("Alice");

    change_name(&name);
}

fn change_name(name: &String) {
    name.push_str(" Smith");
}
```

Why?

Because:

```rust
&String
```

is an immutable reference.

It allows reading, but not changing.

The source shows this situation and explains compiler error `E0596`. fileciteturn0file0L55-L94

---

# 10. Mutable References with `&mut`

If you want to change borrowed data, use `&mut`:

```rust
fn main() {
    let mut name = String::from("Alice");

    add_surname(&mut name);

    println!("Name: {name}");
}

fn add_surname(name: &mut String) {
    name.push_str(" Smith");
}
```

Output:

```text
Name: Alice Smith
```

Notice three things:

```rust
let mut name = ...
```

The original variable must be mutable.

```rust
add_surname(&mut name);
```

We create a mutable reference.

```rust
fn add_surname(name: &mut String)
```

The function accepts a mutable reference.

The source uses the same three changes in its mutable-reference example. fileciteturn0file0L98-L114

---

# 11. `&self` and `&mut self`

The same idea appears in methods.

### `&self`

```rust
fn show(&self)
```

Means:

> Borrow this object so I can read it.

### `&mut self`

```rust
fn change(&mut self)
```

Means:

> Borrow this object so I can change it.

Example:

```rust
struct Student {
    name: String,
    marks: i32,
}

impl Student {
    fn show_marks(&self) {
        println!("Marks: {}", self.marks);
    }

    fn add_marks(&mut self, extra: i32) {
        self.marks += extra;
    }
}
```

---

# 12. Only One Mutable Reference at a Time

Rust has an important rule:

> **You cannot have multiple mutable references to the same data at the same time.**

This is not allowed:

```rust
fn main() {
    let mut number = 10;

    let first = &mut number;
    let second = &mut number;

    println!("{first} {second}");
}
```

Rust reports error `E0499`.

The source explains that the first mutable borrow is still active when the second one is created. fileciteturn0file0L116-L151

---

# 13. Why Does Rust Have This Rule?

Imagine two students changing the same answer on a whiteboard at exactly the same time:

```text
Student A → changes answer
Student B → changes answer
              ↓
           confusion
```

Rust prevents this kind of unsafe access.

Only one mutable reference can control the data at a time.

This helps Rust prevent **data races** at compile time. fileciteturn0file0L151-L159

---

# 14. Multiple Immutable References Are Allowed

You can have multiple read-only references:

```rust
fn main() {
    let number = 10;

    let first = &number;
    let second = &number;

    println!("{first}");
    println!("{second}");
}
```

This is allowed because neither reference can change `number`.

Think:

```text
             ┌── reader 1
             │
number ──────┤
             │
             └── reader 2
```

Multiple readers are safe.

The source confirms that multiple immutable references are allowed because they cannot modify the data. fileciteturn0file0L209-L211

---

# 15. You Cannot Mix Active Immutable and Mutable References

This is not allowed while the immutable references are still being used:

```rust
fn main() {
    let mut number = 10;

    let first = &number;
    let second = &number;

    let third = &mut number;

    println!("{first}, {second}, {third}");
}
```

Why?

```text
first  → reading
second → reading
third  → wants to change
```

Rust does not allow a mutable reference while immutable references to the same value are still active.

This is error `E0502`. fileciteturn0file0L174-L209

---

# 16. References Can Stop Being Used

Rust can determine when a reference is no longer being used.

This is allowed:

```rust
fn main() {
    let mut number = 10;

    let first = &number;
    let second = &number;

    println!("{first} and {second}");

    let third = &mut number;

    *third += 5;

    println!("{third}");
}
```

The immutable references are finished being used after the first `println!`.

Then Rust allows the mutable reference.

The source explains this last-use behavior. fileciteturn0file0L213-L228

---

# 17. What Does `*` Mean?

Suppose:

```rust
let mut number = 10;

let reference = &mut number;
```

The reference points to `number`.

You can use `*` to access the value through the reference:

```rust
fn main() {
    let mut number = 10;

    let reference = &mut number;

    *reference += 5;

    println!("{number}");
}
```

Output:

```text
15
```

Think:

```text
reference
    ↓
number
    ↓
   10

*reference += 5

    ↓

number = 15
```

The source identifies `*` as the dereference operator, the opposite operation of creating a reference with `&`. fileciteturn0file0L27-L29

---

# 18. Dangling References

A **dangling reference** would point to data that no longer exists.

Imagine:

```text
You have a house.
       ↓
You keep its address.
       ↓
House is destroyed.
       ↓
The address is no longer useful.
```

Rust prevents this.

This is invalid:

```rust
fn dangle() -> &String {
    let text = String::from("hello");

    &text
}
```

Why?

`text` belongs to the `dangle` function.

When the function finishes:

```text
text
 ↓
goes out of scope
 ↓
String is dropped
```

But the function is trying to return:

```rust
&text
```

That reference would point to data that no longer exists.

Rust rejects it.

The source explains that Rust guarantees references cannot become dangling references and shows this exact type of invalid code. fileciteturn0file0L232-L301

---

# 19. Correct Way: Return the String

Instead of returning a reference to a local String, return the String itself:

```rust
fn no_dangle() -> String {
    let text = String::from("hello");

    text
}

fn main() {
    let message = no_dangle();

    println!("{message}");
}
```

Output:

```text
hello
```

The String is returned by value, so ownership moves to the caller and the value remains valid. fileciteturn0file0L303-L313

---

# 20. The Two Most Important Borrowing Rules

Memorize these:

```text
RULE 1

Either:

• ONE mutable reference

OR:

• ANY NUMBER of immutable references
```

And:

```text
RULE 2

References must always be valid.
```

The source summarizes these as the core rules of references. fileciteturn0file0L315-L320

---

# 21. Easy Cheat Sheet

| Rust | Simple meaning |
|---|---|
| `value` | Use the value |
| `&value` | Borrow the value |
| `&mut value` | Borrow and allow changes |
| `&String` | Read-only reference to a String |
| `&mut String` | Mutable reference to a String |
| `*reference` | Access the value through a reference |
| `&self` | Read an object |
| `&mut self` | Modify an object |

---

# 22. One Small Practice Program

```rust
fn main() {
    let mut score = 50;

    show_score(&score);

    add_score(&mut score);

    show_score(&score);
}

fn show_score(score: &i32) {
    println!("Score: {score}");
}

fn add_score(score: &mut i32) {
    *score += 10;
}
```

Output:

```text
Score: 50
Score: 60
```

Follow the program:

```text
score = 50
   ↓
&score
   ↓
read score
   ↓
&mut score
   ↓
change score
   ↓
score = 60
```

---

# 23. The Easiest Way to Remember `&`

Whenever you see:

```rust
&value
```

think:

> **"I am borrowing this."**

Whenever you see:

```rust
&mut value
```

think:

> **"I am borrowing this and I can change it."**

Whenever you see:

```rust
&String
```

think:

> **"A borrowed String that I can read."**

Whenever you see:

```rust
&mut String
```

think:

> **"A borrowed String that I can modify."**

---

# 24. Final Mental Model

Think about a school library book:

```text
                 BOOK
                  │
             ┌────┴────┐
             │         │
          Reading    Writing
             │         │
           &book   &mut book
```

Rust's borrowing rules are basically:

```text
Many people can READ the book at the same time.

BUT

Only ONE person can WRITE in the book at a time.

AND

You cannot WRITE while someone is still READING.
```

That simple idea explains most of the borrowing rules you will encounter as a beginner.

---

# Quick Revision

## Borrow without changing

```rust
let name = String::from("Alice");

print_name(&name);

fn print_name(name: &String) {
    println!("{name}");
}
```

## Borrow and change

```rust
let mut name = String::from("Alice");

change_name(&mut name);

fn change_name(name: &mut String) {
    name.push_str(" Smith");
}
```

## Multiple readers

```rust
let a = &value;
let b = &value;
```

Allowed.

## Multiple writers at the same time

```rust
let a = &mut value;
let b = &mut value;
```

Not allowed.

## Reader + writer at the same time

```rust
let a = &value;
let b = &mut value;
```

Not allowed while `a` is still being used.

## Golden rule

```text
Either:

many readers

OR:

one writer

And every reference must point to data that is still valid.
```
