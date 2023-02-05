# UNDERSTANDING OWNERSHIP, ch4

it enables Rust to make memory safety guarntees without needing a garbage collector. 

in this chapter, we will talk about 

* ownership 
* borrowing, slices, how rust lays data out in memory ( stack & heap )



### What is Ownership?

Some languages have garbage collector, and C lang use explicitly allocate and free the memory
but rust use different kind of system.

* Memory is managed through a system of ownership with a set of rules that the compliler checks at compile time. 

| the stack and the heap |
|-|
|All data stored on the stack must have a known, fixed size. Data with an unkown size at compile time or size that might change must be stored on the heap instead|


### Ownership Rules 

* Each value has a variable that's called its owner. 
* There can be only one owner at a time.
* When the owner goes out of scope, the value will be dropped. 

```rust
fn main() {
	{
		let s = "Yeon Kyeong";
		println!("{}", s); 
	}  // s variable will be dropped after this scope 
}
```

* When s comes into scope, it is valid. 
* It remains valid until it goes out of scope. 


### The String Type 

String type is allocated on the heap and as such is able to store an amount of text that is unknown to us at compile time. 

* String 
* str, &String 

the difference is how these two topes deal with memory. 


### Memory and Allocation 

str case, we know this comtents of size at compile time. 

pros -> fast and efficient.

cons -> can not change the value.

* we can not put a blob of memory into the binary for each piece of text whose size is unknown at conpile time and whoese size might change while running the program. 

In other to grow and mutable piece of text, we need to allocate an amount of memory on the HEAP 

* The memory must be requested from the operating system at runtime.
	* ex) String::from().. etc 

* We need to way of returning this memory to the operating system when we are done with our String. 
	* ex) drop 


#### the memory is automatically returned once the variable that owns it goes out of the scope. 

- when the variable goes out of scope, rust program automatically calls the drop method that drops the varable and free the memory. 



### Ways That Variable and Data Interact : Move 

```rust

fn main() {

	let s1 = String::from("hello");
	let s2 = s1; 

}

```

* value 

|s1|status|
|-|-|
|name|value|
|ptr| pointer to heap memory|
|len | 5|
|capacity| 5|

* pointer to heap memory 

|index|value|
|-|-|
|0|h|
|1|e|
|2|l|
|3|l|
|4|o|

- value's pointer is pointing to the heap memory that holding the value "hello".

when s1 is copied to s2 then s1 and s2 have same pointer. 
what if we drop s2 and keep the s1, this will cause memory loss. s1's pointer can not find value that holding in the heap. 

So rust uses "move".

s1 is moved to s2 then s1 is no longer valid. 

### Ways that Variables and Data Interact : Clone 

* Heap data does get coppied. 

### Stack-Only Data : Copy 

if a type have a copy trait, an older variable is still usable after assignment. rust won't allow to use copy trait if the type implement drop trait. 

then what types are Copy ? -> scala type. 

* integer, bool, char, floating, tuple that only contains scala type. 


### Mutable References 

Mutable reference has one bing restriction -> can not allow borrow once it mutably borrow. 

The benefit of having this restriction is that rust can prevent data races at compile time.
A data race is similar to a race condtion and happens when these three behaviors occur :

* Two or more pointers access the same data at the same time. 

* At least one of the pointers is being used to write to the data. 

* There's no mechanism being used to synchronize access to the data. 


### Dangling References 

a pointer that references a location in memory that may have been given to someone else, by freeing some memory while preserving a pointer to that memory. 

* rust ensure that the data can not be dangle when you compile the code. 
* compiler let you know where the dangling occurs. 

### The Rules of References 

* At any given time, you can have either but not both of the following: one mutable reference or any number of immutable references. 

* References must always be valid. 

### The Slice Type 

Another data that does not have ownership is the slice. 
Slices let you reference a contiguous sequence of elements in a collection. 

-> String slices.  its same &String and &str.


### Summary 

Ownership, borrowing, and slices ensure memory safety in Rust at compile time. 











