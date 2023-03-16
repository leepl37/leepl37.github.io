# FUNCTIONAL LANGUAGE FEATRUES : ITERATORS AND CLOSURES, ch 13

functional programming : functions as values by passing them in arguments, returning them from other functions, assigning them to variables for later execution, and so forth. 


we' ll cover:

* Closures, a function like construct you can store in a variable. 

* Iterators, a way of processing a series of elements


pattern matching and enums are influenced by the functional style. Mastering closures and iterators is a key to learn Rust. 



### Closures : Anonymouse functions that can chapture their environment

* rust's closures are value that can contain function. 

* it can be passed as arguments to other functions. 


### Closure type inference and annotation

* function's type annotations are required on functions because they're part of an explicit interface exposed to your users.

* closure definitions will have one concrete type inferred for each of their parameters and for their return value. 
	* types are locked into the closure. 



### Storing closures using generic parameters and the fn traits. 


* memoization or lazy evaluation. 
	* create struct that will hold the closure and the resulting value of calling the closure. the struct will execute the closure only if we need the resulting value, and it will cache the resulting value so the rest of our code doesn't have to be responsible for saving and reusing the result. 





