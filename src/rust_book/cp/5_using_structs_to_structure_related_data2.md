# USING STRUCTS TO STRUCTURE RELATED DATA, ch5

Custome data type. 

Struct and Enums are the building blocks for creating new types in your program's domain to take full advantage of rust's compile time type checking. 

* ### when you use mut keyword on struct, entire instance must be mutable.

* ### using the field init shorthand when variables and field have the same name.

* ### creating instances from other instances with struct update sysntax ( ..user1) 
	* ex) User { ..user1 }; 

* ### using tuple structs without named field to create different types 
	* ex) struct Color(i32, i32, i32);
	* ex) struct Point(i32, i32, i32);

* ### Unit-Like structs without any fields
	* ex) struct A(); 
	* ex) implements trait with unit-like struct is useful. you can distingush with just unit-like struct 


* ### method 
	* ex) impl User { fn method(self, &self, &mut self) }
	* rust has a feature called automatic referencing and dereferencing
		* object.something(), rust automatically adds in &, &mut, or * so object matches the signature of the method. 

		* the fact that rust makes borrowing implicit for method receivers is a big part of making ownership ergonomic in practice. 


* ### associated functions 
	* ex) impl user { fn new() -> something {} } 
	* there no self parameter in associated function, it can be used for making a new instance.. etc 







