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
      - [String Slicing](#string-slicing)
      - [String Formatting](#string-formatting)
      - [Iterating](#iterating)
- [Arrays](#arrays)
  - [Explicit Type Annotation](#explicit-type-annotation-1)
  - [Initialize Array](#initialize-array)
  - [Mutable Arrays](#mutable-arrays)
  - [Array Length](#array-length)
  - [Array Slicing](#array-slicing)
- [Collections](#collections)
  - [Vectors](#vectors)
    - [Adding Values](#adding-values)
    - [Getting Values](#getting-values)
    - [Iterating](#iterating-1)
    - [Multiple Types](#multiple-types)
  - [Hash Maps](#hash-maps)
    - [Adding Pairs](#adding-pairs)
    - [Add Pair if it doesn't exist](#add-pair-if-it-doesnt-exist)
    - [Getting Values](#getting-values-1)
    - [Iterating](#iterating-2)
- [Tuples](#tuples)
  - [Access Tuple Values](#access-tuple-values)
  - [Tuple Destructuring](#tuple-destructuring)
  - [Mutable Tuples](#mutable-tuples)
- [Structs](#structs)
  - [Shorthand Initialization](#shorthand-initialization)
  - [Struct Update Syntax](#struct-update-syntax)
  - [Tuple Structs](#tuple-structs)
  - [Unit-Like Structs](#unit-like-structs)
  - [Strings in Structs](#strings-in-structs)
  - [Methods](#methods)
  - [Associated Functions](#associated-functions)
- [Enums](#enums)
  - [match](#match)
    - [Default](#default)
    - [Simplifying with `if let`](#simplifying-with-if-let)
  - [Enum Values](#enum-values)
  - [The Option Enum](#the-option-enum)
- [Constants](#constants)
  - [Why Constant instead of Immutable Variable?](#why-constant-instead-of-immutable-variable)
- [Operators](#operators)
  - [Borrowing Operator](#borrowing-operator)
  - [Dereferencing Operator](#dereferencing-operator)
- [Conditional Expressions](#conditional-expressions)
  - [If Statements](#if-statements)
    - [Ternaries](#ternaries)
  - [If Let Statements](#if-let-statements)
- [Loops](#loops)
  - [loop](#loop)
  - [for](#for)
  - [Return Values from loops](#return-values-from-loops)
  - [Labeling Loops](#labeling-loops)
- [Ownership](#ownership)
  - [Rules](#rules)
  - [Passing Ownership](#passing-ownership)
  - [References](#references)
    - [Mutable References](#mutable-references)
    - [Dangling References](#dangling-references)
- [Miscellaneous](#miscellaneous)
  - [Input](#input)
  - [Helpful Math Functions](#helpful-math-functions)
    - [int::pow(number, power)](#intpownumber-power)

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

Now, `&str` is used when the string doesn't change and has a fixed width. Similar to a `char` array in C. However, the `String` type allows all the classic modification of strings as one expects from other languages. These `String`s can be created like this:
```rs
let mut my_heap_string = String::from("Hey")
my_heap_string.push_str(" there!")
println!("{}", my_heap_string) // Output: Hey there!
```
Note that `String`s are stored on the heap while `str`s are stored on the stack. Depending on your use case, you'll have to be careful with `String`s.

Also, `str`s can be converting to a `String` like so:
```rs
let my_str = "Hello!";
let my_string = my_str.to_string();
println!("{} {}", my_str, my_string);
// Output:
// Hello! Hello!
```
As you can see, to_string() copies the contents to a `String` type. So both can be used without having to worry about borrowing.

#### String Slicing
String slices are like substrings, but they reference a part of a `String`. They are Python-like in their syntax:
```rs
let hello_world = String::from("Hello, world!");

let hello: &str = &hello_world[..5];
let world: &str = &hello_world[7..12];
println!("{}, {}!", hello, world); // Output: Hello, world!
```
The starting index is inclusive, however, the ending index is not inclusive. The `o` in `Hello` is actually index 4, and the `,` is at index 5. However, the `,` is not included in the slice because the ending index is not inclusive.

Also note that the starting index is missing from `hello`'s slice. This is because that slice starts at the very beginning of the string at index 0. If the string starts at 0, it is not needed and can simply be omitted.

The same can be said for the ending index. If the ending index is omitted, the slice will go from the starting index to the end of the string.

This also means you can omit both and end up with a slice that is the same as the string:
```rs
let hello_world = String::from("Hello, world!");
let slice: &str = &hello_world[..];
println!("{}", slice); // Output: Hello, world!
```

#### String Formatting
String formatting is similar to how `println` is formatted:
```rs
let first_name = "Adam";
let last_name = "Jensen";
let full_name = format!("{} {}", first_name, last_name);
println!("{}", full_name);

// Output:
// Adam Jensen
```

#### Iterating
For `str`s and `String`s alike:
```rs
for char in "Hello!".chars() {
  println!("{}", char);
}

for char in String::from("Hello!").chars() {
  println!("{}", char);
}

//Output for both:
// H
// e
// l
// l
// o
// !
```

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

All the rules of [String slicing](#string-slicing) apply here as well.

# Collections

## Vectors
Vectors are basically arrays stored on the heap, meaning their size can be changed at runtime. To create an empty vector, simply do this:
```rs
let my_vector: Vec<isize> = Vec::new();
```

You can initialize a vector like this:
```rs
let my_vector = vec![1, 2, 3];
```

When a vector is dropped out of scope, **all of its values are also dropped out of scope.** Keep this in mind if you start running into borrowing issues.

### Adding Values
Of course, you must add the `mut` keyword if you actually want to add, remove, or change anything inside the vector. Adding values to a vector is done like so:
```rs
let mut my_vector: Vec<isize> = Vec::new()
my_vector.push(1);
my_vector.push(2);
my_vector.push(3);
```

Note that borrowing a value from a vector and then changing the vector will cause [borrow errors](https://github.com/rust-lang/rust/blob/master/compiler/rustc_error_codes/src/error_codes/E0502.md). So the following code is invalid:
```rs
let mut my_vector = vec![1, 2, 3];
let first = &my_vector[0];
my_vector.push(4); // E0502 here!
println!("The first element is {}", first);
```


### Getting Values
There are two ways to retrieve values from a vector. The classic brackets syntax or with `get(index)`.
```rs
let greetings: Vec<&str> = vec!["Hello!", "Howdy!", "Hey!"];
let first_greeting: &str = greetings[0];
match greetings.get(0) {
  Some(greeting) => println!("{}", greeting),
  None => println!("uh, hi?"),
}
```
`get(index)` is safer than the classic bracket syntax since it handles out of bounds exceptions. If you were to call `greetings[10]`, the code would panic. However, running a `match` on `greetings.get(10)` would simply call the code for `None`.

### Iterating
If you're not changing any values:
```rs
let greetings = vec!["Hello!", "Howdy!", "Hey!"];
for greeting in &greetings {
  println!("{}", greeting);
}

// Output:
// Hello!
// Howdy!
// Hey!
```

If you're changing values:
```rs
let mut nums: Vec<isize> = vec![1, 2, 3];
for num: &mut isize in &mut nums {
  *num += 10;
}
println!("{:?}", nums);
// Output:
// [11, 12, 13]
```

### Multiple Types
Storing multiple types in one vector is impossible. However, there is a workaround. Enums are one type, and each enum variant can store a type. So you could do something like this:
```rs
enum Number {
  Int(isize),
  Float(f64),
}

let nums: Vec<Number> = vec![Number::Int(23), Number::Float(3.14)];
for num in &nums {
  match num {
    Number::Int(value) => println!("{} is an integer", value),
    Number::Float(value) => println!("{} is a decimal number", value),
  }
}

// Output:
// 23 is an integer
// 3.14 is a decimal number
```

## Hash Maps
Hash maps are the standard map type in Rust. You'll need to import a built-in standard library to use hash maps. Here's how to create an empty one:
```rs
use std::collections::HashMap;

let my_map: HashMap<String, isize> = HashMap::new();
```

You can initialize a hash map like this:
```rs
let numbers = HashMap::from([
  ("One", 1),
  ("Two", 2),
  ("Three", 3),
]);
```

### Adding Pairs
You can add entries to the hash map like this:
```rs
let mut numbers = HashMap::new();
numbers.insert("One", 1);
numbers.insert("Two", 2);
numbers.insert("Three", 3);
```

Note that inserting values that don't implement `copy` transfers ownership to the map. So the following code is invalid:
```rs
let key = String::from("One");
let value: isize = 1;
let mut numbers = HashMap::new();
numbers.insert(key, value);
println!("{}", value);
println!("{}", key); // E0382 here!
```
[E0382](https://github.com/rust-lang/rust/blob/master/compiler/rustc_error_codes/src/error_codes/E0382.md) gets thrown when trying to print `key` because `String` does not implement `copy`. However, printing `value` is fine because `isize` implements `copy`. A workaround for this is simply doing this when inserting:
```rs
numbers.insert(key.clone(), value);
```
This will copy the `String` and transfer the copy's ownership rather than the original value.

### Add Pair if it doesn't exist
There are some cases where you may want to add a key-value pair to the map if it doesn't exist. In other languages, you have to check if the map doesn't have the key, and then set the key to a default value. In Rust, it's much easier:
```rs
let mut numbers = HashMap::from([
  ("One", 1),
  ("Two", 2),
  ("Three", 3),
]);
numbers.entry("Three").or_insert(30);
numbers.entry("Four").or_insert(4);
println!("{:?}", numbers);
// {"One": 1, "Two": 2, "Three": 3, "Four": 4}
```
As you can see, `"Three"` didn't get mapped to `30` because it already had a value of `3`. However, `"Four"` was added and mapped to `4` since it didn't exist on the map yet.

### Getting Values
Just like vectors, there's bracket syntax or `get(key)`.
```rs
let numbers = HashMap::from([
  ("One", 1),
  ("Two", 2),
  ("Three", 3),
]);
println!("{}", numbers["One"]);
match numbers.get("One") {
  Some(value) => println!("{}", value),
  None => println!("No number for One"),
}
```
Bracket syntax is less safe than `get(key)` us usual. If a key doesn't exist when using bracket syntax, the code will panic.

### Iterating
```rs
let numbers = HashMap::from([
  ("One", 1),
  ("Two", 2),
  ("Three", 3),
]);
for (key, value) in &numbers {
  println!("{}: {}", key, value);
}

// One: 1
// Two: 2
// Three: 3
```

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

# Structs
Structs are basically just like C. All they contain are public values. However, they can also implement methods as you'll see in [Method Syntax](#method-syntax)
```rs
struct Book {
  name: String,
  author: String,
  page_count: usize,
}

let dune = Book {
  name: String::from("Dune"),
  author: String::from("Frank Herbert"),
  page_count: 412,
};
println!("Name: {}, Author: {}, Pages: {}", dune.name, dune.author, dune.page_count);
// Name: Dune, Author: Frank Herbert, Pages: 412
```

Structs are immutable by default. To make a struct mutable, you mark the variable with `mut` as usual. You cannot make only some properties mutable. Either the entire struct is immutable or the entire struct is mutable.

## Shorthand Initialization
Similar to JavaScript objects, you can use initialization shorthand, as long as the variables have the same name as the struct's properties, like this:
```rs
let name = String::from("Dune");
let author = String::from("Frank Herbert");
let page_count = 412;
let dune = Book { name, author, page_count };
println!("Name: {}, Author: {}, Pages: {}", dune.name, dune.author, dune.page_count);
// Name: Dune, Author: Frank Herbert, Pages: 412
```

## Struct Update Syntax
Take the following structs:
```rs
struct Car {
  make: String,
  model: String,
  year: usize,
  color: String,
}

let blue_camry = Car {
  make: String::from("Toyota"),
  model: String::from("Camry"),
  year: 2014,
  color: String::from("White"),
};

let red_camry = Car {
  make: blue_camry.make,
  model: blue_camry.model,
  year: blue_camry.year,
  color: String::from("Red"),
};
```
Heesh, that's a lot of repetition there, isn't it? Just needed to change one property to make the `red_camry`. Rust thankfully provides what's called the Struct Update Syntax to make this easier and less boilerplate:
```rs
let red_camry = Car {
  color: String::from("Red"),
  ..blue_camry
};
```

## Tuple Structs
So these are a little weird, they're structs that have nameless properties - the properties are accessed like tuples. This allows you to give your tuples names:
```rs
struct Color(u8, u8, u8);

let red = Color(255, 0, 0);
println!("Red: rgb({}, {}, {})", red.0, red.1, red.2);
// Red: rgb(255, 0, 0)
```

## Unit-Like Structs
Defining a tuple struct with no types or properties results in a unit-like struct. As the [Rust docs](https://doc.rust-lang.org/book/ch05-01-defining-structs.html#unit-like-structs-without-any-fields) say, these are useful to implement traits on some type but the trait doesn't have any data associated with it. The trait is simply used as a marker.
```rs
// Example taken from Rust docs linked above
struct AlwaysEqual;

fn main() {
  let subject = AlwaysEqual;
}
```

## Strings in Structs
I used the `String` type rather than the `&str` type in the struct because the struct should own all its values. If you try to use the `&str` type on a struct, error [E0106](https://github.com/rust-lang/rust/blob/master/compiler/rustc_error_codes/src/error_codes/E0106.md) will be thrown.

## Methods
So usually structs simply contain data in other languages. Rust is special - we can implement methods in structs.

Quick note, functions associated with structs are called methods. All methods are functions, but not all functions are methods. A function not belonging to anything (such as the main function) is not a method.
```rs
struct Person {
  first_name: String,
  last_name: String,
}

impl Person {
  fn greeting(&self) {
    println!("{} says: Hello!", self.first_name);
  }
}

let john_doe = Person {
  first_name: String::from("John"),
  last_name: String::from("Doe"),
};
john_doe.greeting(); // John says: Hello!
```
Methods always have the `&self` argument, much like Python. This is simply the instance of the struct that the method was called on.

Methods and properties can share a name, like so:
```rs
struct Person {
  first_name: String,
  last_name: String,
  greeting: String,
}

impl Person {
  fn greeting(&self) {
    println!("{} says: {}", self.first_name, self.greeting);
  }
}

let john_doe = Person {
  first_name: String::from("John"),
  last_name: String::from("Doe"),
  greeting: String::from("HenLo!"),
};
john_doe.greeting(); // John says: HenLo!
println!("{}", john_doe.greeting); // HenLo!
```
Also, it is allowed to have multiple `impl` blocks, although this could be potentially confusing.

## Associated Functions
Associated functions are functions on the struct that don't take the `self` argument (These are not called methods!). They are often used as constructors. A very common associated function that's used a lot is `String::from()`. These are more similar to Java's static functions.
```rs
struct Rectangle {
  width: usize,
  height: usize,
}

impl Rectangle {
  fn square(size: usize) -> Rectangle {
    return Rectangle {
      width: size,
      height: size,
    }
  }
}

let square = Rectangle::square(10);
println!("width: {}, height: {}", square.width, square.height); // width: 10, height: 10
```

# Enums

## match
`match`es are a lot like switch statements in other languages.
```rs
enum Color {
  Red,
  Yellow,
  Green,
}

fn get_rgb(color: &Color) -> (u8, u8, u8) {
  match color {
    Color::Red => (255, 0, 0),
    Color::Yellow => (255, 255, 0),
    Color::Green => (0, 255, 0),
  }
}

println!("{:?}", get_rgb(&Color::Yellow)); // (255, 255, 0)
```

`match`es can of course have blocks:
```rs
fn get_rgb(color: &Color) -> (u8, u8, u8) {
  match color {
    Color::Red => {
      println!("Stop!");
      (255, 0, 0)
    },
    // ...
  }
}
```

### Default
In the switch statements of other languages, they have the default label which is used when the value doesn't match any case given. Rust has this as well in its `match` statements. This is the `_`. In Rust, `_` is a sort of 'unused' marker. It's useful to break out tuples into a single variable, or in this case, to define default behavior if an enum doesn't match any specific case. Take the Color enum from earlier:
```rs
let color = Color::Green;
match color {
  Color::Red => println!("Color is red."),
  _ => println!("Color is not red."),
}

// Output:
// Color is not red.
```

You can also define no behavior if the value doesn't match any desired case:
```rs
match color {
  Color::Red => println!("Color is red."),
  _ => (),
}
```
If `color` is `Color::Red`, then `Color is red.` gets printed. Otherwise, nothing happens.

### Simplifying with `if let`
See [if let statements](#if-let-statements)

## Enum Values
You can store types in enums, similar to Java's enums. However, the value is set at creation of the enum rather than in the enum itself. In other words, the value won't be the same for the same variant of the enum.
```rs
enum Color {
  Red(String),
  Yellow,
  Green,
}

fn get_rgb(color: &Color) -> (u8, u8, u8) {
  match color {
    Color::Red(message) => {
      println!("{}", message);
      (255, 0, 0)
    },
    Color::Yellow => (255, 255, 0),
    Color::Green => (0, 255, 0),
  }
}

let color = Color::Red(String::from("Stop!"));
println!("{:?}", get_rgb(&color));

// Output:
// Stop!
// (255, 0, 0)
```

## The Option Enum
The `Option` enum is Rust's way of having null safety. Null doesn't exist in Rust, but there are some situations where having null is useful. The built-in `Option` enum has only two variants: `Some(T)` and `None`. Here's an example using an `Option` enum to take the absolute value of a number:
```rs
fn abs(num: Option<isize>) -> Option<isize> {
  match num {
    Some(number) => {
      if (number < 0) {
        Some(number * -1)
      } else {
        Some(number)
      }
    }
    None => None,
  }
}

let number = -20;
match abs(Some(number)) {
  Some(value) => println!("Absolute value of {} is {}", number, value),
  None => println!("No value given."),
}

// Output:
// Absolute value of -20 is 20
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

# Loops
While loops are exactly what you expect from a while loop, so I have no notes on those.

## loop
The `loop` keyword allows you to create an infinite loop that only stops on break. Useful for cli applications that continuously wait for commands until the user runs an exit command:
```rs
loop {
  println!("This is the song that never ends");
  println!("it just goes on and on, my friend");
}
```

## for
The `for` syntax is much more `foreach` like in other languages and less C-like. It's always `for loop_variable in collection_or_range`. This makes looping through collections much simpler without the performance loss other languages might have for using this simpler syntax:
```rs
let greetings = ["Hello!", "Hi!", "Hey!", "Good morning!"];
for greeting in greetings {
  print!("{} ", greeting); // Hello! Hi! Hey! Good morning!
}
```
Looping through a range of numbers is still possible, and feels more Python-like. Note that ranges do not include the maximum number:
```rs
for number in 1..5 {
  print!("{} ", number); // 1 2 3 4
}
```
You can still go backwards through a range with `.rev()`:
```rs
for number in(1..11).rev() {
  print!("{}, ", number);
}
println!("Liftoff!");
// 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, Liftoff!
```

## Return Values from loops
`loop`s can return values like so:
```rs
let numbers = [1, 2, 3, 4, 5];
let mut i: usize = 0;
let found_index = loop {
  if numbers[i] == 3 {
      break i;
  }
  i += 1;
};
println!("Found 3 at index {}", found_index); // Output: Found 3 at index 2
```

## Labeling Loops
Just like in Kotlin, you can label loops like this and then break or continue that loop within another nested loop like so:
```rs
'rows for row in (1..4) {
  'columns for column in (1..4) {
    if column == 3 {
      break 'rows
    }
  }
}
```

# Ownership
Now we get into Rust's flagship feature.

## Rules
- Each value has a variable that is called its *owner*.
- There can only be one owner at a time.
- When the owner goes out of scope, the value is dropped.

## Passing Ownership
When you pass a variable into another method, you also pass ownership of that variable.

```rs
fn main() {
  let mut message = String::from("Dear Sam, ");
  print_message(message);
  message.push_str("I'm writing to you about..."); // Compiler error is thrown here
}

fn print_message(message: String) {
  println!("{}", message);
}
```

See, `message` is no longer valid after the `print_message` method because ownership is transferred to that method. Once the method runs, the variable goes out of scope and is dropped. If we wanted to continue using `message` after running `print_message` with it, then we simply call `message.clone()` to pass a copy of `message` into `print_message`:
```rs
fn main() {
  let mut message = String::from("Dear Sam, ");
  print_message(message.clone());
  message.push_str("I'm writing to you about..."); // Compiler is happy with this because printMessage owns a clone of message
}

fn print_message(message: String) {
  println!("{}", message);
}
```

This does not apply to primitives like the integer types, `bool`, float types, `char`, tuples with primitives, and `str`. Primitives automatically clone their values when ownership is to be transferred, so they can continue to be used:
```rs
fn main() {
  let x = 16;
  let y = 4;
  print_sum(x, y);
  let z = x - y; // Compiler is happy with this because x and y have the Copy trait and automatically copy their value since it is so cheap to do so.
}

fn print_sum(num1: i32, num2: i32) {
  println!("{}", num1 + num2);
}
```

## References
References *always* point to a valid value, unlike pointers.

Example from the [Rust docs](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#references-and-borrowing):
```rs
fn main() {
  let s1 = String::from("hello");

  let len = calculate_length(&s1);

  println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
  s.len()
}
```
With a reference, we can still access `s1` after `calculate_length` was run because we didn't transfer ownership to `calculate_length`. We instead gave it a reference.

This is called `borrowing` in Rust.

### Mutable References
Mutable References are, you guessed it, references that can be changed.
```rs
fn main() {
  let mut hello = String::from("Hello");
  finish_sentence(&mut hello);
  println!("{}", hello); // Output: Hello, world!
}

fn finish_sentence(word: &mut String) {
  word.push_str(", world!");
}
```
The only caveat with mutable references is that you can only have a single mutable reference to data at a time. Trying to create two mutable references throws an error. This prevents what Rust calls a **Data Race**. This happens when the following occur:
- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There's no mechanism being used to synchronize access to the data.

We could create a new scope to help ourselves with this:
```rs
let mut x = String::from("Hello, world!");
{
  let y = &mut x;
}
let z = &mut x;
```
This code will compile, because once we come out of the braces, we have exited the scope in which `y` resides. This means that `y` no longer exists and has returned the borrowed mutable reference to `x`. This allows `z` to come and borrow that mutable reference.

One other rule, you cannot create a mutable reference if another immutable reference exists on that data. Again, the idea is to prevent data races.

Something to note, immutable references go into scope when they are called and then go out of scope after their last use. So the following is still valid:
```rs
let mut x = String::from("Hello, world!");
let y = &x;
println!("{}", y);
let z = &mut x;
z.push_str(" Goodnight!");
println!("{}", z);
```
However, this is invalid, and will not compile:
```rs
let mut x = String::from("Hello, world!");
let y = &x;
let z = &mut x;
println!("{}, {}", y, z);
```
Also remember, an infinite amount of immutable references can exist at any time with each other, just not immutable and mutable.

tl;dr, only one mutable reference or an infinite amount of immutable references can exist at once.

### Dangling References
Dangling References do not exist in Rust. While in C++ for example, you can end up with a dangling pointer, Rust will not compile code that could have a dangling reference:
```rs
fn main() {
  let dangling_ref = make_dangle();
}

fn make_dangle() -> &String { // Error: missing lifetime specifier
  let hello = String::from("Hello, world!");
  return &hello;
}
```

# Miscellaneous

## Input
Reading a line of input is processed by the `std::io` library. Here's a full snippet:
```rs
use std::io;

fn main() {
  print!("What is your name?: ");
  let mut name = String::new(); // read_line requires a string *object*. Using a literal creates a reference, String::new() creates an object
  io::stdin()
    .read_line(&mut name)
    .expect("Failed to read line");
  println!("Hello, {}!", name.trim()); // The input string will also contain the newline at the end that the user had to enter in order to submit the input
}
```

## Helpful Math Functions

### int::pow(number, power)
Takes the `number` to the power of `power`:
```rs
println!("{}", u8::pow(2, 3)); // Output: 8
```
