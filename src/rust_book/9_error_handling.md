# ERROR HANDLING, Ch 9

we will cover..

* recoverable vs unrecoverable 

* panic! macro and return Result<T, E> values. 

* deciding whether to try to recover from an error or to stop execution. 


### unrecoverable Errors with panic! 

when panic! macro executes, your program will print a failure message, unwind and clean up the stack, and the quit. but this process is a lot of work. the alternative is to immediately abort, which ends the program without cleaning up. 

at *Cargo.toml* file. 


|[profile.release]|
|-|
|panic = 'abort'|

* when we try to get the value of the vector that is out of index, std lib vec, calls panic! macro. 
	* like C language, it can access the memory that is not included in vector array it cause a lot of problem.
	* use RUST_BACKTRACE=1 cargo run to see details of error where is it cause. 



### recoverable Errors with Result. 

```rust

enum Result<T, E> {
	
	Ok(T),
	Err(E),
	
}

```

in this Result struct, 

* T represents return type of a success case -> Ok(T).

* E represents return type of a fail case -> Err(E). 



```rust
      
      File::open("Yeon.txt"); 

      // returns Ok(file) or Err(e) it is implemented in std lib, fs::File, io::ErrorKind.

```

### shortcuts for Panic on Error : unwrap and expect 

* unwrap will return the value inside the Ok. if the value inside Result is Err(e) then it will call the panic! macro. 

* using .expect("messages") convey your intent and easy to track down the source of a panic. 


### propagating the error 

```rust 
	
	fn read_file() -> Result<io::File, io::Error> {
		File::open("yeon")
	}

```

* the function that call read_file() can receive the Result<T, E>. 
	* we can choose what we will do after receving the result type. 



### a shortcut for propagating errors : the ? operator. 
```rust 

fn read_username_from_file() -> Result<String, io::Error> {
   
    let mut f = File::open("hello.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)

}

   // chaining also works 
   
   File::open("hello.txt")?.read_to_string(&mut s)?;

```

* ? convert automatically. 

* ? operator can only be used in function that return result type. 
	* Result<(), Box<dyn Error>> is called a trait object we will talk about this later. 




### To panic! or Not to panic! 

* prototype code, and test panic is very useful. 

* cases in which you have more information than the compiler -> you can choose *unwarp* because you know what the result type is. 


### Guidelines for error handling 

* the bad state is not something that's expected to happen occationally. 

* your code after this point needs to rely on not being in this bad state. 

* there's not a good way to encode this information in the types you use. 



### Summary 

* the panic! macro signals that your program is in a state it can not handle and lets you tell the process to stop instead of trying to proceed with invalid or incorrect value. 

* the result enum uses rust's type system to indicate that operations might fail in a way that your code could recover from. 
	* it needs to handle potential success or failur. 






