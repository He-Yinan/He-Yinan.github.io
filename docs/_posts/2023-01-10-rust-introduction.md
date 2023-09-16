---
layout: post
title:  "Rust Notes"
date:   2023-01-10 23:11:21 +0800
categories: jekyll update
tags: rust programming-language
---
Rust is a System programming language like C/C++. They are used to build software and software platforms. They can be used to build operating systems, game engines, compilers...

These programming languages require a great degree of hardware interaction.

Rust focuses on:

1.   Safety
2.   Speed
3.   Concurrency

The language was designed for developing highly reliable and fast software in a simple way. Rust can be used to write high level programs down to hardware-specific programs.

## Installation on Mac

```shell
$ curl https://sh.rustup.rs -sSf | sh
$ source $HOME/.cargo/env
```

## Hello World

1.   Open a text editor and write the code

     ```rust
     fn main(){
        println!("Hello World");
     }
     ```

2.   Compile the code

     ```shell
     rustc Hello.rs
     ```

Upon successful compilation of the program, an executable file is generated.

## Data Types

Rust is a statically typed language. Every value in Rust is of a certain data type. The compiler can automatically infer data type of the variable based on the value assigned to it.

### Declare Variables

Use the `let` keyword to declare a variable.

```rust
fn main() {
   let company_string = "TutorialsPoint";  // string type
   let rating_float = 4.5;                 // float type
   let is_growing_boolean = true;          // boolean type
   let icon_char = '♥';                    //unicode character type

   println!("company name is:{}",company_string);
   println!("company rating on 5 is:{}",rating_float);
   println!("company is growing :{}",is_growing_boolean);
   println!("company icon is:{}",icon_char);
}
```

-   `{}` is a placeholder

The placeholder will be replaced by the variable's value.

### Scalar Types

A scalar type represents a single value. Rust has 4 scalar types.

1.   Integer
2.   Floating-point
3.   Booleans
4.   Characters

### Automatic Type Casting

Automatic Type Casting is **NOT** allowed in Rust. The following code will cause the compiler to throw a mismatch types error.

```rust
fn main() {
   let interest:f32 = 8;   // integer assigned to float variable
   println!("interest is {}",interest);
}
```

## Variables

A variable is a named storage that programs can manipulate. Variables in Rust are associated with a specific data type. The data type determines the size and layout of the variable's memory. The range of values that can be stored within that memory and the set of operations that can be performed on the variable.

### Variable Declaration

No type specified

```rust
let variable_name = value;
```

Type specified

```rust
let variable_name:dataType = value
```

### Immutable

By default, variables are immutable in Rust. A variable's value cannot be changed once a value is bound to a variable name.

The following code will cause the compiler to throw an error.

```rust
fn main() {
   let fees = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```

Cannot assign values twice to immutable variable.

### Mutable

To make a variable mutable, prefix the variable name with `mut` keyword.

The following code can be compiled successfully.

```rust
fn main() {
   let mut fees:i32 = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```

### Shadowing

Rust allows programmers to declare variables with the same name. In such a case, the new variable overrides the previous variable.

-   Rust support variables with different data types while shadowing.

```rust
fn main() {
   let salary = 100.00;
   let salary = 1.50 ; 
   // reads first salary
   println!("The value of salary is :{}",salary);
}
```

## Constants

Constants represent values that cannot be changed. If you declare a constant, then there is now way its value changes. The keyword for using constants is `const`.

-   Constants MUST be explicitly typed.
-   Declared using `const` and not `let`.
-   Can be set only to a constant expression and not to the result of a function call or any other value that will be computed at runtime.
-   Does not support shadowing.

```rust
const VARIABLE_NAME:dataType = value;
```

## String

The string data type in Rust can be classified into the following:

1.   String Literal (&str)
2.   String Object (String)

### String Literal

String literal are used when the value of a string is known at compile time. String literals are a set of characters, which are hardcoded into a variable.

-   Also known as string sliced.

```rust
fn main() {
   let company:&str="TutorialsPoint";
   let location:&str = "Hyderabad";
   println!("company is : {} location :{}",company,location);
}
```

### String Object

The string object type is provided in Standard Library. Unlike string literal, the string object type is not a part of the core language. It is defined as a public structure in standard library `pub struct String`. String is a growable collection. It is mutable and UTF-8 encoded type. The string object type can be used to represent string values that are provided at runtime.

-   Allocated in the **heap**.

```rust
fn main(){
   let empty_string = String::new(); // empty string object created
   println!("length is {}",empty_string.len());

   let content_string = String::from("TutorialsPoint");
   println!("length is {}",content_string.len());
}
```

Empty string can be initialized by appending the string with a value using the `push_str()` function.

## Control Flow

-   If statement

```rust
if num > 0 {
   println!("number is positive") ;
}
```

-   If else statement

```rust
if num % 2==0 {
   println!("Even");
} else {
   println!("Odd");
}
```

-   Match statement

    The default case is  `_` 

```rust
let state_code = "MH";
let state = match state_code {
   "MH" => {println!("Found match for MH"); "Maharashtra"},
   "KL" => "Kerala",
   "KA" => "Karnadaka",
   "GA" => "Goa",
   _ => "Unknown"
};
```

-   For loop

```rust
fn main(){
   for x in 1..11{ // 11 is not inclusive
      if x==5 {
         continue;
      }
      println!("x is {}",x);
   }
}
```

-   While loop

```rust
fn main(){
   let mut x = 0;
   while x < 10{
      x+=1;
      println!("inside loop x value is {}",x);
   }
   println!("outside loop x value is {}",x);
}
```

-   Loop

    Same as `while(true)`.

```rust
fn main(){
   //while true
   let mut x = 0;
   loop {
      x+=1;
      println!("x={}",x);

      if x==15 {
         break;
      }
   }
}
```

## Functions

A function is a set of statements to perform a specific task. Once defined, functions may be called to access code. A function declaration tells the compiler about a function's name, return type, and parameters. A function definition provides the actual body of the function.

### Function Definition

```rust
fn function_name(param1, param2) -> return_type {
   //statements
   return value;
}
```

### Pass by Value

When a method is invoked, a new storage location is created for each value parameter. The values of the actual parameters are copied into them. 

Changes made to the parameter inside the invoked method have no effect on the argument.

```rust
fn main(){
   let no:i32 = 5;
   mutate_no_to_zero(no);
   println!("The value of no is:{}",no); // 5
}

fn mutate_no_to_zero(mut param_no: i32) {
   param_no = param_no*0;
   println!("param_no value is :{}",param_no); // 0
}
```

### Pass by Reference

When a parameter is passed by reference, **NO** new storage location is created for the parameter. The reference parameters represent the same memory location as the actual parameters that are supplied to the method.

-   Parameter values can be passed by reference by prefixing the variable name with an `&`.

```rust
fn main() {
   let mut no:i32 = 5;
   mutate_no_to_zero(&mut no);
   println!("The value of no is:{}",no); // 0
}
fn mutate_no_to_zero(param_no:&mut i32){
   *param_no = 0; //de reference
}
```

-   The `*` operator is used to access value stored in the memory location that the variable param_no points to.
