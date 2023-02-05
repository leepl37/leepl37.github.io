# ENUMS AND PATTERN MATCHING, Ch6

### this chapter covers... 

* how an enum can encode meaning along with data 

* explore particulary useful enum, called Option 

* how pattern matching in the match expression 

* if let construct

#### Rust's enums are most similar to algebraic data types in functional languages, such as F#, OCaml, and Haskell. 


### defining and using enum 

```rust
fn main() {
	enum IpAddrKind {
		V4, // or V4(u8, u8, u8, u8) 
		V6, // or V6(String),
	}

	let four = IpAddrKind::V4; // or IpAddrKind::V4(127, 0, 0, 1);
	let six = IpAddrKind::V6; // or IpAddrKind::V6(String::from("::1"));
}

```

* example of enum 

	enum Msg {
		Quit,
		Move { x: i32, y: i32}, 
		Write(String), 
		ChangeColor(i32, i32, i32),
	}

 	-> Msg Enum whose variants each store different amounts and types of values.

	* Quit has no data associated with it at all. 
	* Move includes an anonymous struct inside it. 
	* Write includes a single String. 
	* ChangeColor includes a three i32 values 



* why are we using enum?
 -> if we use different structs, which each have their own type, we couldn't as easily define a fucntion. 



### The Option Enum and Its Advantages over Null Values. 

* Expressing this concept in terms of the type system means the compiler can check whether you've handled all the cases you should be handling. 


* Rust does not have null feature. 
	-> the problem with null values is that if you try to use null value as  not-null value, you'll get an error of some kind. 

	-> the concept that null is trying to express is still a useful one: a null is value that is currently invalid or absent for some reason. 


* Rust does have an enum that can encode the concept of a value being present of absent. 

```rs
enum Option<T> {
	Some(T),
	None, 
}
	
	let some_number = Some(5);
	let some_string = Some("a string");
	let absent_number : Option<i32> = None; 
```

* it included in the prelude. 
* you can you Some or None directly. 
* if we use None rather than Some, we need to tell Rust what type of Option<T> we have because compiler can not infer the type. 
* can not use value inside some directly, we need to take the value from Some<T> 

### The match Control Flow Operator 

```rs 
	let some_val = Some(123);

	match some_val {
		Some(t) -> println!("in some, there is value : {}", t),
		_ -> println!("there is no value at all"), // or we can use use (), 
		
	};

```

* in this case, uses '_' in match this means it can take all cases that aren't specified before it. 
* () is just the unit type, nothing will happen in the _ case. 



### if let 

```rust
	let some_val = Some(123);

	if let val = some_val {
		println!("there is vale inside some_val : {}", val);
	}else{
		println!("none");
	}

```



### Sommary 

* how to create custom enum type. 
* Option<T> 
* match & if let 



