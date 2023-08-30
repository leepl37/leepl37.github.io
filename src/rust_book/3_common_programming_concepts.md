# Common Programming Concepts, ch3

## Summary:
this chapter introduces essential programming concepts that are common not only in Rust but also in many other languages.
The chapter covers variables, data types, functions, comments, and control flow.



### key Concepts:

#### 1.Variables and Mutablility

* `Variables` and `Mutability`:
	* `Variables` are immutable by default in Rust. 
	* To make a variable mutable, use the `mut` keyword.

##### Example:

```rust
fn main() {
	
	
	let x = 5; // Immutable variable
	
	let mut y = 6; // Mutable variable

	// x += 1; //Not allowed.
	y += 1; //Allowed because y is mutable

	println!(" result of `y` : {}", y);

}
```


#### 2. Data Types:

* Scalar - integers, floating-point numbers, Booleans and characters. 
	* int - signed, unsigned : 8 ~ 128 bit and isize, usize. 
* Compound - tuples, arrays 
	* Can take user-defined `struct` and `enumeration`. 

```rust
fn main() {
	
	let a: i32 = 10;
	
	let b = 3.14; // Type inference 

	let x : i32 = -1;

	let y : u32 = 1;

	let pi: f64 = 3.14159265359;

	let is_true : bool = true;

	let c : char = 'a';

	//Tuples : can contain mutiple values of different types.
	let person : (i32, f64, &str) = (25, 5.9, "Ho");

	//Arrays : only contains the same type.
	let numbers : [i32, 3] = [3,2,1];
}

```

#### 3. Functions: 

* Functions are defined using the `fn` keyword.
* Parameters must have their types explicitly annotated.

```rust 

fn add(x : i32, y : i32) -> i32 {
	 x + y 
}

fn main() {
	 println!(" x + y = {}", add(3, 5));
}

```

#### 4. Comments: 

* Single-line comments start with `//`
* Multi-line comments are rarely used but start with `/*` and end with `*/`.

```rust 

fn main() {
	
	// This is a single-line comment

	/* This is a 
		 muti-line comment */

}
```

##### 5. Control Flow:
* `if-else`, `loop`, `while`, and `for` are available for control flow.


##### 6. Statements and Expressions

* Statements: These are instructions that perform some action and do not return a value. They usually end with a semicolon(`;`).

* Expressions : These are pieces of code that evaluate to a value and do not end with a semicolon. 
	* Expressions can be part of a statement. 

```rust

fn main() {

		let x = 5; //Statement : This initializes the variable `x` with the value 5.

		let y = (x + 1); // Expression : ` x + 1 ` evaluates to a value (6 in this case).
		
		
		let x1 = 5;  // statement

		let y1 = {
				let z = 3;  // statement
				z + 1 // expression
		}; // this block is also an ??

		println!(" y = {}", y1);


}

```

