# MANAGING GROWING PROJECTS WITH PACKAGES, CREATES, AND MODULES

What we will covers .. 

As a project grows, you can organize code by splitting it into multiple modules and then multiple files. 
a package can contain multiple binary crates and optionally one library crate. 
as package grows, you can extract parts into separate crates that become external dependencies. 

* organizing project. 
* grouping functionality, encapsulating implementation details lets you reuse code at a higher level. 
* parts of code are public or private. 


### Module system 

* Packages : a cargo feature that lets you build, test, share crates. 
* Crates : a tree of modules that produces a library or executable. 
* Modules and use : let you control the organization, scope, and privacy of paths. 
* Path : a way of naming an item, such as a struct, function, or module. 



### Packages and crates 

* a crate is a binary of library. (root file is library:lib.rs, binary:main.rs) 
* rust compiler starts from and makes up the root module of your crate. 
* one or more crates that provide a set of functionality. 
* package must contain zero or one library crates and no more. 
* it can contain as many binary crates as you'd like, but it must contain at least one crate(either lib, bin)
* a package can have multiple binary crates by placing files in the src/bin directory : each file will be separate binary crate. 
* crate's functionality is namespaced in its own scope. ex) rand::Rnd 


### Defining Modules to Control Scope and Privacy 

* we will discuss :
	* use keyword.
	* pub keyword.
	* as keyword, external packages 
	* the glob operator 


in the src/lib.rs 

we difined a module. 

```rust
	
	mod frond_of_house {
		mod hosting {
			fn add_to_waitlist() {}

			fn seat_at_table() {}
		}

		mod serving {
			fn take_order(){}

			fn serve_order(){}

			fn take_payment(){}
		}
	}


```


* we difined a module start with mod keyword.

* inside modules, we can have other modules and also hold definitions for other items, such as structs, enums, constant, triat, and function. 

* modules can be useful when you navigate the code you do not need to follow all the code but just follow the groups in this case module. 

* the etire module tree is rooted under the implicit module named crate. 

* lib.rs/main.rs are called crate roots -> it looks like tree structure, filesystem ex) crate root / frond_of_house / hosting / add_to_waitlist. it look like this. 


#### Paths for Referring to an Item in the Module Tree 

A path can take two forms : 

* An absolute path : starts from a crate root by using a crate name or a literal crate.   

* An relative path : starts from the current module and uses self, super, or an identifier in the current module. 


these path forms are followed by one or more identifiers separated by double colons (::). 

```rust
	
	mod frond_of_house {
		mod hosting {
			fn add_to_waitlist() {}
		}
	}



	pub fn eat_at_restaurant() {
	
	//absolute path 
	crate::front_of_house::hosting::add_to_waitlist();

	//relative path 
	front_of_house::hosting::add_to_waitlist(); 

	}

```


when to use relative path and absolute path?

* The decision should depend on whether you're more likely to move item definition code separately from together with the code that uses the item. 

	* if we mode the front_of_house module and the eat_at_restaurant function into a module named customer_experience, we'd need to update the absolute path to add_to_waitlist, but the relative path would still be valid. 


* modules do -

	* organizing your code 
	
	* define rust's privacy boundary 
		
		* private is default in rust. 

		* Items in a parent module can not use the private items inside child modules. 
		
		* Items in child module can use the items in their ancestor modules. 
			* the reason is that child modules wrap and hide their implementation details, but the child modules can see the context in which they're defined. 




### Exposing Paths with the pub Keyword. 


```rust
	
	mod frond_of_house {
		pub mod hosting {
			pub fn add_to_waitlist() {}
		}
	}



	pub fn eat_at_restaurant() {
	
	//absolute path 
	crate::front_of_house::hosting::add_to_waitlist();

	//relative path 
	front_of_house::hosting::add_to_waitlist(); 

	}

```

* adding the pub keyword to mod hosting and fn add_to_waitlist lets us call the function from eat_at_restaurant.




### Starting Relative Paths with super. 

src/lib.rs
```rust

fn serve_order(){}

mod back_of_house {
	fn fix_incorrect_order() {
		cook_order(); 
		super::serve_order(); // relative path with super. 
	} 

	fn cook_order() {}
}
```

-> we used super so we'll have fewer places to update code in the future if this code gets moved to a different module. 


### Making Structs and Enums Publics 

if we make struct public, but the struct's fields will still be private. 
```rust
	// src/lib.rs 
	mod back_of_house {
		pub struct Breakfast {
			pub toast: String, 
			seasonal_fruit: String, 
		}
		
		impl Breakfast {
			pub fn summer(toast: &str) -> Breakfast {
				Breakfast {
					toast : String::from(toast), 
					seasonal_fruit : String::from("peaches"),
				}
			}

		}
	}


	pub fn eat_at_restaurant() {
		let mut meal = back_of_house::Breakfast::summer("Rye");
		// Change our mind about what bread we'd like 
		meal.toast = String::from("wheat");
		// but seasonal_fruit can not be modified. 
	}

```

In contrast, if we make an enum public, all of its variants are then public. 






### Bringing Paths into Scope with the use Keyword 

```rust
	//   src/lib.rs 
	
	mod front_of_house {
		pub mod hosting {
			pub fn add_to_waitlist() {}
		}
	}

	use crate::front_of_house::hosting;
	// we can also bring module with relative path. 
	// use front_of_house::hosting;

	pub fn eat_at_restaurant() {
		hosting::add_to_waitlist();
	}

```


* adding use and a path in a scope is smilar to creating a symbolic link in the filesystem. 


### Creating Idiomatic use Paths. 

you might wonder why we use 'use create::front_of_house::hosting' instead of bring it all the way out to add_to_waitlist.  


* bring it all the way out to fn is unidiomatic. 

* otherwise we can have benefit. 

	* when calling the function, we can catch the function isn't locally defined. 
	
	* Bringing two types with the same name into the same scope requires using their parent module otherwise we can not ditingush which one is which. 

	* on the other hand, when bringing in structs, enums, and other items with use, it's idiomatic to specify the full path. 

	* there's no string reason behind this idiom: it's just the convention that has emerged, and folks have gotten used to reading and writing Rust code this way. 


### Providing New Names with the as Keyword. 

```rust
	use std::io::Result as IoResult; 
```


### Re-exporting Names with pub use. 

To enable the code that calls our code to refer to that name as if it had been defined in that code's scope, we can conbine pub and use. 

* this technique is called 're-exporting' 

	* making that item available for others to bring into their scope. 

```rust

	// src/lib.rs 

	mod front_of_house {
		pub mod hosting {
			pub fn add_to_waitlist(){}
		}
	}
	
	pub use crate::front_of_house::hosting;

```

By using pub use, external code can now call the add_to_waitlist function using hosting::add_to_waitlist. 
if we had not specified pub use, it can be called in the scope but external code could not. 

* Doing so makes our library well organized for programmers working on the library and programmers calling the library. 

### Using External Packages. 

in the Cargo.toml 

[dependencies]
rand = "0.5.5"

```rust
use rand::Rng;

fn main() {
	let secret_number = rand::thread_rng().gen_range(1,101);
}

```


Note that standard library (std) is also a crate that's external to our package. 
it shiped with rust, so we do not need to include it. 

### Using Nested Paths to Clean Up Large use Lists. 

```rust 

// use std::io;
//use std::cmp::Ordering; 

// it can also be used this way. 

use std::{io, cmp::Ordering}; 

// use std::io; 
// use std::io::Write; 

use std::io::{self, Write}; 

```


### The Glob Operator 

```rust
	
use std::collections::*; 

```

glob can make it harder to tell what names are in scope and where a name used in your program was defined. 

The glob operators is often used when testing to bring everything under thest into the tests moduile. 


### Separating Modules into Different Files. 

move the front_of_house module to its own file src/front_of_house.rs 

```rust
// src/lib.rs 

mod front_of_house; 

pub use crate::front_of_house::hosting; 

```


```rust
// src/front_of_house.rs 

pub mod hosting {
	pub fn add_to_waitlist() {}
}

```


Using a semicolon after mod front_of_hose rather than using a block tells Rust to load the contents of the module fron another file with the same name as the module. 


same example as follws... 

```rust
// src/front_of_house.rs 
pub mod hosting; 
```

```rust
// src/front_of_house/hosting.rs 
pub fn add_to_waitlist(){}
	
```

this technique lets you move modules to new files as they grow in size. 



### Summary 

* organize your packages into crates 

* your crates into modules so you xan refer to items defined in one module from another module. 

* using a relative path or absolute path denpend on the situation. 

* bring it into the scope with a use statement. 

* module code is private by default, use pub keyword to public usecases.


