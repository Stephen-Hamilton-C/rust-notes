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

Typically, Rust follows the [snake_case](https://en.wikipedia.org/wiki/Snake_case) naming convention, much like C++.

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
