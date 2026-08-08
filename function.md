# Functions

📘 Functions in Rust
In Rust, functions are declared using the `fn` keyword. They allow you to write reusable blocks of code and are a fundamental building block of any Rust program.

🔹 Basic Syntax

```rust
fn function_name(parameter1: Type, parameter2: Type) -> ReturnType {
    // function body
    return value; // optional, you can also just end with the expression
}
```
🔹 Example

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b  // no `return` or semicolon needed for final expression
}
```
🔹 Calling a Function

```rust
fn main() {
    let result = add(5, 3);
    println!("Result is: {}", result);
}
```

🔹 Key Points
- Return Type: Defined using `-> Type`. If not specified, the return type is `()`, meaning "unit" (like void in other languages).
- Implicit Return: The last expression is returned automatically (no semicolon).
- Parameters: Must declare types (x: i32).
- Functions must be declared before use, or declared above `main()` or imported.

🔹 Functions with No Parameters

```rust
fn greet() {
    println!("Hello!");
}
```
🔹 Functions Returning Nothing

```rust
fn log_message(msg: &str) {
    println!("LOG: {}", msg);
    // return type is () by default
}
```
🔹 Function with Multiple Return Paths

```rust
fn check_number(x: i32) -> &'static str {
    if x > 0 {
        "Positive"
    } else if x < 0 {
        "Negative"
    } else {
        "Zero"
    }
}
```


## Programs

```rust
#[allow(unused_variables)]
fn main(){
    openstore("Brooklyn");
    bakepizza(2, "pepperoni", "cheese");
    openstore("Queens");
    openstore("Princes");
    openstore("Markow");
    bakepizza(5, "pepperoni", "mushroom");
    bakepizza(7, "cheese", "corn");
    let result: i32 = square(3);
    println!("The square of 3 is {result}");

    let result: i32 = square(13);
    println!("The square of 13 is {result}");

    let multiplier = 5;
    
    let calculation : i32 = { // isolation of code 
        let value: i32 = 5 + 4;
        value * multiplier
    };
    println!("{calculation}")
}

fn openstore(neighborhood: &str) {
    println!("Opening my Store in {neighborhood}");
}
fn bakepizza(number: i32, topping1: &str, topping2: &str){
    println!("Baking {number} Pizza's with {topping1} and {topping2}");
}

fn square(number: i32) -> i32 { //here -> is used to return the value of i32 that has to be mentioned
    //return number * number; // return ends the square function
    number * number
}
```
