# Object Oriented programming, Ch 17

a trait object points to both an instance of a type implementing our specified trait as well as a table used to look up trait methods on that type at runtime. 

```rust, editable

// we can move this code to lib.rs like a modules. 

pub trait Draw {
	
	fn draw(&self);

}


pub struct Screen {

	pub components : Vec<Box<dyn Draw>>,
	// pub components : Vec<T>, -> it can not take multiple types at a time.  
}


impl Screen {

	fn run(&self) {

		for component in self.components.iter() {
			
			component.draw(); 

		}
	}

}


struct SelectBox {

	// elided .. 
}

impl Draw for SelectBox {
	
	fn draw(&self) {
	 // elided ..
	 println!(" draw for select box");
	}
}


struct Button {

	// elided .. 
}

impl Draw for Button {
	
	fn draw(&self) {
	 // elided .. 
	 println!(" draw for button");
	}
}

fn main() {

	let screen = Screen {
		components : vec![
			Box::new(SelectBox{
				// ..
			}),
			Box::new(Button{
				// .. 
			}), 
		],
	};

	screen.run(); 

}


```



* A generic type parameter can only be substituted with one concrete type at a time, whereas trait objects allow for multiple concrete types to fill in for the trait object at runtime.



### Trait Objects Perform Dynamic Dispatch

* recall in performance of code using generic, monomorphization performed by the compiler.  -> static dispatch 

* dynamic dispatch cases, the compiler emits codes that at runtime will figure out which method to call. 

* trait object have two pointer, one is the pointer of the instance of struct or enum that implements the trait andthe other pointer points to the funtions that implemeted on the instance. (compiler checks that the type of object and if its safe, then create pointer address and put it in the vtable).

* dynamic dispatch also prevents compiler from inline a method's code, which in turn prevents some optimizations. 



### Object Safety is required for trait objects 

* the return type is not self. (Clone trait) 

* there are no generic type parameters. 


### example 


```rust, editable 

fn main() {

trait State {

	// we use *Box<Self>*, invalidating the old state so the state value of **Post** can transform into a new state.   
    fn request_review(self: Box<Self>) -> Box<dyn State>;
    
    fn approve(self: Box<Self>) -> Box<dyn State>;
    
    fn content<'a>(&self, post: &'a Post) -> Option<&'a str> {
      Some("") 
    }

    fn reject(self: Box<Self>) -> Box<dyn State>;

}

#[derive(Debug)]
struct Draft {}

impl State for Draft {
   
   fn approve(self: Box<Self>) -> Box<dyn State> {
	self
   }
   
   fn request_review(self: Box<Self>) -> Box<dyn State> {
   	Box::new(PendingReview {})
   }
   
   fn reject(self: Box<Self>) -> Box<dyn State> {
	self
   }

    fn content<'a>(&self, post: &'a Post) -> Option<&'a str> {
        None 
    }
}
 

#[derive(Debug)]
struct PendingReview {}

impl State for PendingReview {
  
	fn approve(self: Box<Self>) -> Box<dyn State> {
		Box::new(Published {})
   	}
    
	fn request_review(self: Box<Self>) -> Box<dyn State> {
		self
	}
	
	fn reject(self: Box<Self>) -> Box<dyn State> {
		Box::new(Draft {})
   	}

    fn content<'a>(&self, post: &'a Post) -> Option<&'a str> {
        Some("") 
    }
}


#[derive(Debug)]
struct Published {}

impl State for Published {
	fn request_review(self: Box<Self>) -> Box<dyn State> {
		self
	}
 
 	fn approve(self: Box<Self>) -> Box<dyn State> {
		self
 	}

 	fn content<'a>(&self, post: &'a Post) -> Option<&'a str> {
        println!("Published contents");
		Some(&post.content)
	}
	
	fn reject(self: Box<Self>) -> Box<dyn State> {
		self
   	}
}

// #[derive(Debug)]
pub struct Post {
    state: Option<Box<dyn State>>,
    content: String,
}

// impl Debug for Post {}

impl Post {
    pub fn new() -> Post {
        Post {
          state: Some(Box::new(Draft {})),
          content: String::new(),
        }
   }

    pub fn add_text(&mut self, text: &str) {
        match self.state.as_ref().unwrap().content(&self) {
            Some(s) => println!("add text : {:?}", s),  
            None => {
                self.content.push_str(text);
            },
        }
    }


    // take method to take ownership of it. 
    pub fn request_review(&mut self) {
	if let Some(s) = self.state.take() {
        	self.state = Some(s.request_review())
        }
    }

    pub fn approve(&mut self) {
        if let Some(s) = self.state.take() {
            self.state = Some(s.approve())
        }
    }

    pub fn content(&self) -> &str {
        match &self.state.as_ref().unwrap().content(self) {
            Some(s) => s,
            None => "",  
        }
    	// self.state.as_re().unwrap().content(&self).unwrap()
    }

    pub fn reject(&mut self) {
		if let Some(s) = self.state.take() {
			self.state = Some(s.reject())
	}
    }

  }

    let mut post = Post::new(); 
    println!("{:?}", post.content());
    post.add_text("test");
    println!("{:?}", post.content());
    post.request_review();
    post.add_text("hello");
    println!("{:?}", post.content());
    post.approve();
    post.add_text("world");

    println!("####### {:?}", post.content());

}

```


###  Trade-offs of the State Pattern 

we've shown that Rust is capable of implementing the object-oriented state pattern to encapsulate the different kinds of behavior a post shold have in each state. 


the state pattern can result in some coupling between states, as each state object is responsible for transitioning to the next state. Additionally, there may be some duplication of logic, and attempts to eliminate this may violate object safety. Finally, implementing the state pattern in Rust as it is defined for object-oriented languages may not take full advantage of Rust's strengths, such as compile-time checks for invalid states and transitions.

Therefore, the state pattern in Rust is a trade-off between the benefits of encapsulating state-based behavior and the potential downsides of coupling between states and duplication of logic. By using Rust's unique features such as macros and compile-time checks, it is possible to mitigate some of these downsides and create more efficient and maintainable code.


### Encoding states and behavior as types 


the code below shows that it will be impossible for us to accidentally display draft post content in production, because that code won't even compile. 

```rust, editable

pub struct Post {
    content: String,
}

 pub struct DraftPost {
    content: String,
}

 impl Post {
 
 pub fn new() -> DraftPost {
         DraftPost {
             content: String::new(),
         }
 }

   pub fn content(&self) -> &str {
          &self.content
     }
}
 
 impl DraftPost {
   pub fn add_text(&mut self, text: &str) {
         self.content.push_str(text);
    }
}


```


* draftpost does not have a content method, so it can display the content.

### implementing transition as transformations into different types. 


```rust, editable

fn main(){

pub struct Post {
    content: String,
}

 pub struct DraftPost {
    content: String,
}

 impl Post {
 
 pub fn new() -> DraftPost {
         DraftPost {
             content: String::new(),
         }
 }

   pub fn content(&self) -> &str {
          &self.content
     }
}
 
 impl DraftPost {
   pub fn add_text(&mut self, text: &str) {
         self.content.push_str(text);

	}
   pub fn request_review(self) -> PendingReviewPost {
        PendingReviewPost {
            content: self.content,
        }
    }
}
 
pub struct PendingReviewPost {
    content: String,
}
 
impl PendingReviewPost {
    pub fn approve(self) -> Post {
        Post {
            content: self.content,
        }
   }
}
    

let mut post = Post::new();
 post.add_text("I ate a salad for lunch today");
 let post = post.request_review();
 let post = post.approve();
 assert_eq!("I ate a salad for lunch today", post.content());

}




```



Rust is capable of implementing objects-oriented patterns, other patterns, such as encoding state into the type system, are available in Rust. 


### summary 

 Rust can use trait objects to implement some object-oriented features and patterns, which can improve code maintainability at the expense of runtime performance. 
 Rust has other unique features such as ownership that traditional object-oriented languages lack, and these features should be considered when choosing the best design pattern. 


