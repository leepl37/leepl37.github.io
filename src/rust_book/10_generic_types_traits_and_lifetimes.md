# GENERIC TYPE, TRAITS, AND LIFETIMES, Ch, 10

Generics are abstract stand-ins for concrete types or other properties. 

we will expore..

* how to define your own types, functions, and methods with generics. 

* how to use generic type in struct and enum definitions. 

* how to use traits to define behavior in a generic way. 
	* combine traits with generic types to constrain a generic type to only those types that have a particular behavior. 

* lifetimes, a variety of generics that give the compiler imformation about how reference relate to each other. lifetimes allow us to borrow values in many situations while still enabling the compiler to check that the references are valid. 



### In Function Definitions 

we place the generics in the signature of the function. where we would usually specify the data types of the parameters and return value. 

#### ex) fn function<T>(param : T) -> T {}, this function is generic over some type T.	

* you can use any type identifier as a type parameter name. But 'T' by convention, parameter names in Rust are short, often just a letter, and Rust's type-naming convention is CamelCase. "type", T is the default choice of most Rust programmers. 


### In Struct Definitions 

```rs 
struct Point<T> {
	x : T,
	y : T,
}

struct Differ_field<T, U> {
	x : T, 
	y : U,
}
```

### In Enum Definitions 

```rs 

enum Option<T> {
	Some(T), 
	None, 
}

enum Result<T, E> {
	Ok(T), 
	Err(E), 
}


```
### In Method Definitions 


```rs 
struct Point<T> {
	x : T,
	y : T,
}

impl<T> Point<T> {
	fn x(&self) -> &T {
		&self.x
	}
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}

```

* By declaring T as a generic type after impl, Rust can identify that the type in the angle brakets in Point is a generic type rather than a concrete type. 

* we could implement concrete type method for Point struct. 


### Performance of Code Using Generics

Monomorphization is the process of turning generic code into specific code by filling in the concrete types that are used when compiled. 

* we pay no run time cost for using generics. 

### Traits: Defining Shared Behavior. 

* tells the rust compiler about functionality a particular type has and can share with other type. 

* trait bounds to specify that a generic can be any time that has certain behavior. 

* group method signatures. 

* accomplish some purpose. 



```rs 

trait Summary {
	fn summary(&self) -> String; 
}

```

* implementing this trait, compiler enforce you to implement all methods trait has. 

* we do not need to curly brakets for implement function, we use semiconlon instead. 

* we use alse curly brakets to implement function. ex) default fucntion.. 


### Implementing a Trait on a Type. 


```rs 

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}
 impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}

```



* we can implement a trait on a type only if either the trait or the type is local to our crate. 
* this restriction is called, coherence, orphan rule. ( parent type is not present ) 

* two crates could implement the same trait for the same type, and Rust wouldn’t know which implementation to use.


### Default Implementaion. 

```rs

pub trait Summary {

   fn summarize_author(&self) -> String; 

   fn summarize(&self) -> String {
     String::from("(Read more...)")
     }
     }

```

* Default implementations can call other methods in the same trait, even if those other methods don’t have a default implementation.

```rs 

impl Summary for Tweet {
    fn summarize_author(&self) -> String {
      format!("@{}", self.username)
    }
}

```

* Note that it isn’t possible to call the default implementation from an overriding implementation of that same method.


### Traits as Parameters. 

```rs 

pub fn notify(item: impl Summary) {
    println!("Breaking news! {}", item.summarize());
}


// trait bound syntax 

pub fn notify<T: Summary>(item: T) {
    println!("Breaking news! {}", item.summarize());
}

// where clauses 

fn some_function<T, U>(t: T, u: U) -> i32
     where T: Display + Clone,
           U: Clone + Debug
{
   ...
}

```




### Returning Types that implement Traits. 

The ability to return a type that is only specified by the trait it implements is especially useful in the context of closures and iterators, which we cover in Chapter 13.

```rs 

fn returns_summarizable(switch: bool) -> impl Summary {
    if switch {
        NewsArticle {
            headline: String::from("Penguins win the Stanley Cup Championship!"),
            location: String::from("Pittsburgh, PA, USA"),
            author: String::from("Iceburgh"),
            content: String::from("The Pittsburgh Penguins once again are the best
            hockey team in the NHL."),
        }
    } else {
        Tweet {
            username: String::from("horse_ebooks"),
            content: String::from("of course, as you probably already know, people"),
            reply: false,
            retweet: false,
       }
 }
}

```


* this code would not work. 

* due to restrictions around how the impl Trait syntax is implemented in the compiler. We’ll cover how to write a function with this behavior in “Using Trait Objects That Allow for Values of Different Types”



### Using Trait Bounds to Conditionally Implement Methods. 

```rs 

struct Pair<T> {
    x: T,
    y: T,
}
 impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self {
            x,
            y,
        }
   }
}
 impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
          println!("The largest member is y = {}", self.y);
        }
    }
}

```

* blanket implementations. 


#### example of std. 

```rs 

// the ToString trait on any type that implements the Display trait. 

impl<T: Display> ToString for T {
    // --snip--
}

```

* we can call to_string method defined by the ToString Trait on any type that implements the Display trait. 


* Blanket implementations appear in the documentation for the trait in the “Implementors” section.

* Traits and trait bounds let us write code that uses generic type parameters to reduce duplication but also specify to the compiler that we want the generic type to have particular behavior.

* In dynamically typed languages, we would get an error at runtime if we called a method on a type that the type didn’t implement. But Rust moves these errors to compile time so we’re forced to fix the problems before our code is even able to run.



