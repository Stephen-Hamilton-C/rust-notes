# rust-notes <!-- omit in toc -->
Sandbox to learn about Rust

- [Cargo](#cargo)
  - [New Project](#new-project)
  - [Build](#build)
  - [Clean](#clean)
  - [Run](#run)
- [Comments](#comments)
  - [Documentation](#documentation)
- [Macros](#macros)
  - [println!](#println)
    - [Placeholders](#placeholders)
  - [eprintln!](#eprintln)
- [Variables](#variables)
  - [Declaration](#declaration)
  - [Mutable](#mutable)
  - [Declaring Multiple Variables](#declaring-multiple-variables)
  - [Explicit Type Annotation](#explicit-type-annotation)
    - [Integers](#integers)
    - [Decimals](#decimals)
    - [Strings](#strings)
- [Arrays](#arrays)
  - [Explicit Type Annotation](#explicit-type-annotation-1)
  - [Initialize Array](#initialize-array)
  - [Mutable Arrays](#mutable-arrays)
  - [Array Length](#array-length)
  - [Array Slicing](#array-slicing)
- [Tuples](#tuples)
  - [Access Tuple Values](#access-tuple-values)
  - [Tuple Destructuring](#tuple-destructuring)
  - [Mutable Tuples](#mutable-tuples)
- [Constants](#constants)
  - [Why Constant instead of Immutable Variable?](#why-constant-instead-of-immutable-variable)
- [Operators](#operators)
  - [Borrowing Operator](#borrowing-operator)
  - [Dereferencing Operator](#dereferencing-operator)
- [Miscellaneous](#miscellaneous)
  - [Input](#input)
  - [Helpful Math Functions](#helpful-math-functions)
    - [int::pow(number, power)](#intpownumber-power)
- [Conditional Expressions](#conditional-expressions)
  - [If Statements](#if-statements)
    - [Ternaries](#ternaries)
  - [If Let Statements](#if-let-statements)

# Cargo
Cargo is the package manager, much like JavaScript's NPM or Yarn and Java's Gradle or Maven.
Thankfully, Cargo is the official package manager for Rust, which likely means there won't be any others to cause slight discrepencies.

## New Project
Creating a new Rust project is easy. You can run `cargo init` in an existing directory to turn it into a Cargo Package,
or you can run `cargo new <path>` if the directory doesn't exist yet. Additionally, `cargo new` will initialize a Git repo.

## Build
`cargo build` will build the project. By default, the binary executable is located at `./target/debug/package_name`.
The `package_name` is determined by the `name` attribute in `Cargo.toml`.

## Clean
`cargo clean` will remove any build artifacts.

## Run
`cargo run` runs the built executable. Code starts in `./src/main.rs`'s main() method

# Comments
Comments are exactly like C

## Documentation
Documentation comments are different. Unfortunately (imo), Rust went away from the uniform JavaDoc standard that almost every language is using now.
"Outer Doc" comments are written outside a block of code and are denoted with `///`. Markdown is supported. "Inner Doc" comments are written inside a block of code and are denoted with `//!`.

------------------------ FIXME: Need example ------------------------

# Macros
Macros basically indicate injected code. They are always denoted with a `!` suffix. The most common macro is `println!()`

## println!
Prints a line to terminal. Example: 
```rs
println!("Hello, world!");
```

### Placeholders
Placeholders allow for some neat formatting. Not as powerful as Kotlin's `$` notation, but still helpful. Unfortunately I don't think this applies to anything but printing, but I haven't tested this yet.

`{}` denotes that the braces should be replaced with the next argument in sequence. Example:
```rs
println!("Hello, {}!", "Bob"); // Output: Hello, Bob!
```

More placeholders will go further down the sequence. Example:
```rs
println!("Hello, {} {}!", "Bob", "Page"); // Output: Hello, Bob Page!
```

You can determine which argument gets used by specifying the index in between the braces, like this:
```rs
println!("Your name is {0}. How do you do, {0}?", "Bob"); // Output: Your name is Bob. How do you do, Bob?
```

## eprintln!
Outputs to standard error. Example:
```rs
eprintln!("You cannot do that!");`
```

# Variables
Variables are declared similar to TypeScript and are **immutable** by default.

Typically, Rust follows the [snake_case](https://en.wikipedia.org/wiki/Snake_case) naming convention, much like C++. But of course, you can follow whichever naming convention you prefer. However, if you don't use snake_case, Rust will harass you with warnings.

## Declaration
Initializing a variable looks like this:
```rs
let my_var = "Hello, world!";
```
As you can see, types can be inferred by their initialized values and variables are strictly typed. I cannot assign an `int` to `my_var` because it is of type `string`.

## Mutable
As stated earlier, variables are **immutable** by default. To declare a mutable variable, use the `mut` keyword:
```rs
let mut my_var = "Hello, world!";
my_var = "Goodbye!"; // This won't throw an error because my_var is mutable.
```

## Declaring Multiple Variables
You can declare multiple variables at the same time, much like Lua:
```rs
let (last_name, first_name) = ("Page", "Bob");
println("{}, {}", last_name, first_name); // Output: Page, Bob.
```

## Explicit Type Annotation
You can of course explicitly annotate types in the same style as TypeScript:
```rs
let my_bool: bool = true;
```
Type names are mostly like C.

### Integers
You cannot simply use `int` to declare an integer. You must indicate how many bits the int should use, such as `i32` for signed integers, or `u32` for unsigned integers. As you might assume, the `32` indicates how many bits the variable uses. It can be 8, 16, 32, 64, or 128.

Additionally, the int type can also be `isize` or `usize`. If the computer is 32 bit, it'll occupy 32 bits. If the computer is 64 bit, it'll occupy 64 bits. You get the gist.

Example:
```rs
let mut my_int: u8 = 255; // 255 is the maximum value for an unsigned 8-bit integer.
my_int = 256; // This will cause a compile-time error because 256 is larger than 8 bits.
```

### Decimals
Floats and doubles cannot be declared with `float` or `double`. Instead, you can declare floats with `f32` and doubles with `f64`.

Example:
```rs
let my_float: f32 = 0.125;
let my_double: f64 = 0.0078125;
```

### Strings
Explicitly typing strings is a little different:
```rs
let my_string: &str = "Hello, world!";
```
The `&` indicates the variable is a reference, much like C++. A string literal is a reference to that literal, so when explicitly typing a string that is initialized with a literal, you'll need that `&`.

# Arrays

## Explicit Type Annotation
Type inference exists for arrays as well, but they can of course be explicitly typed too, with the format `[type; size]`:
```rs
let greetings: [&str; 3] = ["Hello, world!", "Hello, Gordon!", "Look Gordon, ropes!"];
```

## Initialize Array
You can initialize an array rapidly, similar to JavaScript's Array.fill():
```rs
let my_array = [0; 3];
println!("{:?}", my_array); // Output: [0, 0, 0]
```

## Mutable Arrays
When an array is mutable, its values can be changed. It's size *cannot* be changed:
```rs
let mut mutable_array = [true; 3];
mutable_array[2] = false;
println!("{:?}", mutable_array); // Output: [true, true, false]
```

## Array Length
Getting the length of an array is always slightly different in every language:
```rs
let my_array = [0; 3];
println!("Array length: {}", my_array.len()); // Output: Array length: 3
```

## Array Slicing
You can get references to portions of an array:
```rs
let big_array: [u8; 100] = [0; 100];
let small_slice: &[u8] = &big_array[0..5];
println!("slice length: {}", small_slice.len()); // Output: slice length: 5
```
Slices do not include the last index.

# Tuples
Essentially an array with different types in fixed locations.
```rs
let name_and_age: (&str, u8) = ("Isaac", 54);
println!("{:?}", name_and_age); //Output: ("Isaac", 54)
```

## Access Tuple Values
Tuples actually use a `.` rather than `[]` to get their values, like so:
```rs
let name_and_age = ("Isaac", 54);
println!("Name: {}, Age: {}", name_and_age.0, name_and_age.1); // Output: Name: Isaac, Age: 54
```

## Tuple Destructuring
You can destructure a tuple in one line like this:
```rs
let name_and_age = ("Isaac", 54);
let (name, age) = name_and_age;
println!("Name: {}, Age: {}", name, age); // Output: Name: Isaac, Age: 54
```

## Mutable Tuples
Just like arrays, you can make tuples mutable so their values can be edited:
```rs
let mut name_and_age = ("Isaac", 54);
name_and_age.1 = 55;
println!("{}", name_and_age.1); //Output: 55
```

# Constants
Naming conventions for constants in Rust are typically [SCREAMING_SNAKE_CASE](https://en.wikipedia.org/?title=SCREAMING_SNAKE_CASE). Although again, choose whichever naming convention works best for you. No one's stopping you. However, you should expect Rust libraries to be written with these conventions.

## Why Constant instead of Immutable Variable?
- Constants are set at compile-time and cannot have any runtime values inside them, for example, this will throw an error:
```rs
let sum = 2 + 2;
const FOUR = sum; // While sum is always 4, this had to be determined after compile time, so an E0435 error was thrown.
```
- Constants are both local and global scope when declared, whereas standard variables are only local scope.
- Constants cannot be made mutable with the `mut` keyword.
- The type *must* be explicitly declared for constants.
- Constants cannot be shadowed

# Operators
I won't go over the simple stuff like logical, arithmetic, bitwise, assignment, and type casting as they're the same as any other language.

## Borrowing Operator
There are two types of borrowing: **Shared Borrowing**, and **Mutable Borrowing**. Shared Borrowing is readonly while Mutable Borrowing can be shared and altered by a single variable, however the data will be inaccessible to other variables at that time.

Shared borrowing:
```rs
let x: u8 = 10;
let y: &u8 = &x;
println!("x: {}, y: {}", x, y); // Output: x: 10, y: 10
```

Mutable Borrowing:
```rs
let mut x: u8 = 15;
let y: &mut u8 = &mut x;
println!("y: {}", y); // Output: y: 15
```

## Dereferencing Operator
The dereferencing operator gets the value of a referenced variable, much like C pointers:
```rs
let mut x: u8 = 15;
let y: &mut u8 = &mut x;
println!("y: {}", y); // Output: y: 15
*y = 20;
println!("y: {}", y); // Output: y: 20
println!("x: {}", x); // Output: x: 20
```

# Miscellaneous

## Input
Reading a line of input is processed by the `std::io` library. Here's a full snippet:
```rs
use std::io;

fn main() {
  println!("What is your name?");
  let mut name = String::new(); // read_line requires a string *object*. Using a literal creates a reference, String::new() creates an object
  io::stdin().read_line(&mut name).expect("Failed to read line");
  println!("Hello, {}!", name.trim()); // The input string will also contain the newline at the end that the user had to enter in order to submit the input
}
```

## Helpful Math Functions

### int::pow(number, power)
Takes the `number` to the power of `power`:
```rs
println!("{}", u8::pow(2, 3)); // Output: 8
```

# Conditional Expressions
I won't go into detail on the simple stuff, I'll simply point out some differences between the standard C-like syntax we're all used to and Rust's specific syntax

## If Statements
If statements don't require parenthesis to be wrapped around their expressions. Both of these if statements are valid:
```rs
let on = true;
if on {
  turnOn();
}
if (!on) {
  turnOff();
}
```
However, the Rust compiler will give warnings if you give unnecessary parenthesis.

### Ternaries
Ternaries are similar to Kotlin's:
```rs
let age = 18;
let message = if age >= 18 { "You're an adult!" } else { "You're not an adult yet." };
println!("{}", message);
```

## If Let Statements
If let statements do pattern matching with scrutinee expressions. After the `let`, you define a pattern and then an `=` and a scrutinee expression. A common use that I found is comparing enums:
```rs
enum Foo {
  Bar,
  Baz
}

let bar = Foo::Bar;

if let Foo::Bar = bar {
  println!("a is Bar");
}

// This will output "a is Bar"
```
