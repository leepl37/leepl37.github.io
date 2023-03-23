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

	* Memoization refers to the technique of caching the results of a function call so that subsequent calls with the same arguments can be returned quickly from the cache instead of recomputing the result. This can be useful in cases where a function is computationally expensive or has side effects that can be avoided with caching. Memoization can be implemented using a HashMap or a similar data structure in Rust.

Lazy evaluation, on the other hand, refers to the evaluation of an expression only when its value is actually needed, rather than eagerly evaluating it before it is needed. This can be useful for optimizing performance and reducing memory usage in cases where not all values need to be computed or stored at once. In Rust, lazy evaluation can be implemented using closures, iterators, and the lazy_static crate.

Both memoization and lazy evaluation can be powerful techniques for optimizing Rust code, but they are best used judiciously and in cases where they provide a clear benefit.



 ```rs 

struct Cacher<T> 
	where T: Fn(u32) -> u32 {
		calculation : T, 
		value : Option<u32>, 
	}


 ```




* Note 

	*functions can implement all three of the Fn traits, too. If what we want to do doesn not require capturing a value from the environment, we can use a function rather than a clousre where we need something that implements an Fn trait*



```rs 
impl<T> Cacher<T>
  ➊ where T: Fn(u32) -> u32
{
  ➋ fn new(calculation: T) -> Cacher<T> {
      ➌ Cacher {
             calculation,
             value: None,
         }
     }
 ➍ fn value(&mut self, arg: u32) -> u32 {
         match self.value {
         ➎ Some(v) => v,
         ➏ None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
           },
         }
  }
}


```



```rs 


fn generate_workout(intensity: u32, random_number: u32) {
  ➊ let mut expensive_result = Cacher::new(|num| {
         println!("calculating slowly...");
         thread::sleep(Duration::from_secs(2));
         num
     });


    if intensity < 25 {
         println!(
             "Today, do {} pushups!",
          ➋ expensive_result.value(intensity)
         );
         println!(
             "Next, do {} situps!",
          ➌ expensive_result.value(intensity)
         );
     } else {
         if random_number == 3 {
             println!("Take a break today! Remember to stay hydrated!");
         } else {
             println!(
                 "Today, run for {} minutes!",
              ➍ expensive_result.value(intensity)
             );
	 }
    }
}


```




### Limitations of the cacher implementation 

* problem is that the first time we called c.value with 1, the Cacher instance saved Some(1) in self.value. Thereafter, no matter what we pass in to the value method, it will always return 1.

* to fix this problem using a hash map -> the key will the are **arg** values that are passed in, and the value of key will be the result of caluation. 



### Capturing the environment with closures 

```rs 

fn main() {

	let x = 4; 

	let equal_to_x = |z| z == x; 

	let y = 4; 

	assert!(equal_to_x(y));

}


```

* when a closure captures a value from its environment, it uses memory to store the values for use in the closure body

* but this case is overhead so we do not want to pay in more common cases where we want to execute code such as funtions. 



#### three ways of capturing 

* taking ownership, borrowing mutably, and borrowing immutably. 


	* **FnOnce** : taking ownership of a variable, it can be called only once. 

	* **FnMut** : mutably borrows values, can change the environment. 

	* **Fn** : borrows values from the environment immutably. 

* rust infers which trait to use based on how the closure uses the values from the environment. 

* take ownership of the values, move keyword force to take the value of ownership. 

	* this technique is mostyly useful when passing a closure to a new thread to move the data so it's owned by the new thread 



```rs 

fn main() {
 
	 let x = vec![1, 2, 3];
	 
	 let equal_to_x = move |z| z == x;
	 
	 // this print macro does not work because x has been moved. 
	 println!("can't use x here: {:?}", x);

	 let y = vec![1, 2, 3];
	 
	 assert!(equal_to_x(y));
}

```

### Processing a Series of Items with Iterators 










