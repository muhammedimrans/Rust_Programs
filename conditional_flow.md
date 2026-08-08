Conditional Statements 

✅ Rust Conditional Statements
Rust provides familiar conditional control flow with some Rust-specific strictness and features.

🔹 if, else if, and else

```rust
fn main() {
    let age = 18;

    if age < 13 {
        println!("Child");
    } else if age < 18 {
        println!("Teen");
        } else {
        println!("Adult");
    }
}
```

📌 Key Points:
* Conditions must return bool.
* No parentheses () are required around the condition.
* Curly braces {} are mandatory for all blocks, even single-line ones.

🔸 Using if as an Expression

```rust
fn main() {
    let score = 85;
    let grade = if score >= 90 {
        "A"
    } else if score >= 80 {
        "B"
    } else {
        "C"
    };

    println!("Grade: {}", grade);
}
```

📌 Key Points:
* if returns a value and can be used to assign to a variable.
* All branches must return the same type.

🔹 match Statement (Pattern Matching)

```rust
fn main() {
    let number = 3;

    match number {
        1 => println!("One"),
        2 => println!("Two"),
        3 => println!("Three"),
        _ => println!("Something else"),
    }
}
```

📌 Key Points:
* match is like a switch but more powerful.
* _ is the default fallback pattern.
* All arms must be exhaustive (handle all cases).

🔸 match as an Expression

```rust
fn main() {
    let status_code = 404;

    let message = match status_code {
        200 => "OK",
        404 => "Not Found",
        500 => "Internal Server Error",
        _ => "Unknown",
    };

    println!("Status: {}", message);
}
```
match can return a value and be assigned to variables.

🔹 if let – Conditional Matching

```rust
fn main() {
    let config = Some("Enabled");

    if let Some(value) = config {
        println!("Config is set to {}", value);
    } else {
        println!("Config is not set");
    }
}
```

📌 Key Points:
* if let is shorthand for matching one specific pattern.
* Useful for Option, Result, enums, etc.

🔸 while let – Looping with Pattern Matching

```rust
fn main() {
    let mut stack = vec![1, 2, 3];

    while let Some(top) = stack.pop() {
        println!("Top: {}", top);
    }
}
```
Continues loop while pattern matches (e.g., while Some).

🧠 Summary

* if / else	Basic conditional branching, Must be boolean; braces required
* if expression	Assign values based on condition, All branches must return same type
* match	Pattern-based conditional control, Exhaustive and powerful
* if let	Match one pattern cleanly	Useful for Option and Result
* while let	Loop while matching a pattern, Great for stack/queue processing

🔹 break

The break statement exits the loop immediately, regardless of the loop condition.
✅ Example:

```rust
fn main() {
    let mut count = 0;

    loop {
        if count == 5 {
            break; // Exit the loop
        }
        println!("Count is: {}", count);
        count += 1;
    }
}
```
🖨 Output:

Count is: 0
Count is: 1
Count is: 2
Count is: 3
Count is: 4

🔹 continue

The continue statement skips the current iteration and moves to the next loop cycle.
✅ Example:

```rust
fn main() {
    for i in 1..6 {
        if i == 3 {
            continue; // Skip when i == 3
        }
        println!("i = {}", i);
    }
}
```
🖨 Output:

i = 1
i = 2
i = 4
i = 5

🔸 break with continue (used in loop)
In Rust, break and continue are used inside loops like for, while, or loop. Here's how they work:

🔁 What is a Loop?
A loop is a way to repeat some code again and again.

Example:
```rust
for number in 1..5 {
    println!("{}", number);
}
```
 This will print:
1234
 🚪 break – Stop the loop immediately
Imagine you're in a class, and the teacher says:
"Keep reading until I say STOP."
That STOP is like break in Rust. It exits the loop early.

Example:

```rust
fn main() {
    for number in 1..10 {
        if number == 5 {
            break; // stops the loop when number is 5
        }
        println!("{}", number);
    }
}
```
 Output:
1234
🧠 Explanation: When number becomes 5, break runs and the loop ends.

⏭️ continue – Skip to the next loop
Imagine you're checking homework pages 1 to 10, but you skip page 5.
That "skip" is like continue — it skips that one turn but keeps going.

Example:
```rust
fn main() {
    for number in 1..6 {
        if number == 3 {
            continue; // skip when number is 3
        }
        println!("{}", number);
    }
}
```
Output:
1245
🧠 Explanation: Number 3 is skipped, but the loop continues.

✅ Summary
Keyword	What it does
break	Stops the loop completely
continue	Skips the current step, moves to next

🔸While loop
🌀 What is a while loop?
A while loop keeps running again and again as long as a certain condition is true.

It’s like saying:
“While I am hungry, I will keep eating.”

In Rust, it looks like this:

```rust
while condition {
    // code to repeat
}
```
 🧠 How does it work?
It checks the condition (like x < 5)
If the condition is true, it runs the code inside the loop.
Then it goes back and checks the condition again.
If the condition becomes false, the loop stops.

✅ Example 1: Counting from 1 to 5

```rust
fn main() {
    let mut number = 1;

    while number <= 5 {
        println!("Number is: {}", number);
        number += 1;
    }
}
```
📋 What happens?
Starts at 1
Prints: Number is: 1
Adds 1 → becomes 2
Checks: is 2 ≤ 5? Yes → continue
Keeps going until it prints 5 
🔸 What is Recursion?
Recursion means a function calling itself to solve a smaller version of the same problem.
Imagine you have to count down from 5 to 1:
* You say "5"
* Then "4"
* Then "3"
* And so on...
Instead of using a loop (for or while), we can use recursion to do this.

📘 Simple Recursion Example in Rust: Countdown

```rust
fn countdown(n: i32) {
    if n == 0 {
        println!("Done!");
    } else {
        println!("{}", n);
        countdown(n - 1); // function calls itself with a smaller number
    }
}

fn main() {
    countdown(5);
}
```
🧾 What this does:
* countdown(5) prints 5, then calls countdown(4)
* countdown(4) prints 4, then calls countdown(3)
* This keeps happening until n == 0, then it prints "Done!"

🧮 Another Example: Factorial
Factorial of 5 (written as 5!) is:

5! = 5 × 4 × 3 × 2 × 1 = 120
We can calculate this using recursion!

```rust
fn factorial(n: u32) -> u32 {
    if n == 0 {
        1
    } else {
        n * factorial(n - 1)
    }
}

fn main() {
    let result = factorial(5);
    println!("Factorial of 5 is: {}", result);
}
```
🧾 Explanation:
* factorial(5) is 5 * factorial(4)
* factorial(4) is 4 * factorial(3)
* ...
* factorial(1) is 1 * factorial(0)
* factorial(0) returns 1, and everything multiplies back up

🔐 Important Things to Remember:
1. Base Case: You must stop the recursion somewhere (like if n == 0) or it will run forever and crash.
2. Smaller Step: Always move toward the base case (e.g., n - 1).
3. Rust is safe: It checks to make sure your program doesn’t use too much memory when doing recursion.
