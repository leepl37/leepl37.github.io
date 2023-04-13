# Fearless Concurrency, Ch 16


* How to create threads to run multiple pieces of code at the same time. 

* *Message-Passing* concurrency, where channels send messages between threads. 

* *Shared-state* concurrency, where multiple threads have a access to some piece of data. 

* The **Sync** and **Send** traits, which extends Rust's concurrency guarntess to user-defined types as well as types provied by the standard library


### thread 

Spliting the computation in your program into multiple threads can improve performance but it also cause

* Race conditions : accesing data or resource in an inconsistent order. 

* Deadlocks : two threads are waiting for each other to finish using a resource the order thread has. 

* Bugs : happen only in certain situations and hard to fix. 


Rust in std only provide 1:1 threading. (check M:N threading and what is green thrad).


example)

* the return type of *thread::spawn* is **Join Handle** (it owns value). 
* *join* method on it, will wait for its thread to finish. 


```rust, editable 
use std::thread;
use std::time::Duration;

fn main() {

	let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi number {} from the spawned thread!", i);
            thread::sleep(Duration::from_millis(1));
 		}
    	});
	
	for i in 1..5 {
        	println!("hi number {} from the main thread!", i);
        	thread::sleep(Duration::from_millis(1));
    	}
     	handle.join().unwrap(); // -> block the current thread. 
}

```


* join on the handle blocks the thread currently running until thread represented by the handle terminates. 

* *Blocking* a thread means that thread is prevented from performing work or exiting.


### Using move Closures with Threads. 

transfer data from one thread to another thread. 


```rust, editable

fn main() {
	let v = vec![1, 2, 3];
	
	let handle = thread::spawn(|| {
		println!("Here's a vector: {:?}", v);
		});

	// here 
	handle.join().unwrap();
}

```

* can not compile because the thread is borrowing value v but compiler does not know how long spawned thread will run.


* example, if we put drop code above *drop(v)* at **here** then thread using v value has a problem.

* use *move* keyword can fix these problem.


### Using message passing to transfer data between threads.

* *channel* has two halves : *transmitter* and *receiver* . 

* a chanel is said to be *closed* if either transmitter or receiver half is dropped.


usecase : chat system, or a system where many threads perform parts of a calulation and send the  parts to one thread that aggregates the results. 


```rust, editable  

use std::sync::mpsc;
use std::thread;

fn main(){

	let (tx, rx) = mpsc::channel(); 

	thread::spawn(move || {
		let tranfer_value = String::from("value");
		let o = tx.send(tranfer_value).unwrap();
		println!("{:?}", o);
	});

	//let recv = rx.recv().unwrap();
	let recv = rx.recv();
	println!("{:?}", recv);

}


```

* mpsc stands for *multiple producer, single consumer*. 

	* multiple streams flowing together into one big river.

* *send()* method returns Result<T, E> type, if receiver is dropped results will be Result<E> otherwise it returns nothing. 

* *recv()* method block the current threads and returns Result<T, E> type, if sending end of the channel closes, recv will return error otherwise it returns value.

* the receiving end of a channel has two useful methods : recv(), try_recv().

* try_recv() methods does not block, but it will instead return Results immediately: an Ok or Error if there are any messages this time. 

	* using it is useful when thread has other work to do while waiting for messages. 




```rust, editable

use std::thread;
use std::sync::mpsc;
use std::time::Duration;
fn main() {
	let (tx, rx) = mpsc::channel();

	let tx1 = mpsc::Sender::clone(&tx);
	thread::spawn(move || {
  		let vals = vec![
	        	String::from("hi"),
	        	String::from("from"),
	        	String::from("the"),
	        	String::from("thread"),
  			];
		
		for val in vals {
       			tx1.send(val).unwrap();
       			thread::sleep(Duration::from_secs(1));
			}
		});


	thread::spawn(move || {
		let vals = vec![
		   String::from("hi"),
		   String::from("from"),
		   String::from("the"),
		   String::from("thread"),
		];
		for val in vals {
		   tx.send(val).unwrap();
		   thread::sleep(Duration::from_secs(1));
		}
		});

	for received in rx {
		println!("Got: {}", received);
	}
}


```

* *multiple producer single consumer* 

### Shared-State Concurrency 

mulitiple threads can access the same memory location at the same time. 

* mutexes, one of the more common concurrency primitives for shared memory. 

	* *Mutex* , *mutual exclusion* .

* to access the data in a mutex, a thread must signal that it wants access by asking to acquire the mutax' *lock*. 

* the mutex is descrived as *garding* the data it holds via locking system. 

**mutex rules**

* you must attempt to acquire the *lock* before using the data. 

* when you're done with the data that the mutex guards, you must unlock the data so other thread can acquire the lock. 

	* example of pannel disscusion at conference with only one microphone. 



```rust, editable

use std::sync::Mutex;

fn main() {

	let m = Mutex::new(5);
	
	{
		println!("lock = {:?}", m.lock());
		
		let mut num = m.lock().unwrap();
	
		println!("num = {:?}", num);
		
		*num = 100;
	}

	println!("m = {:?}", m);

}


```

* *lock* will block the current thread until the thread had held lock releses the lock. 

* the call to *lock* would fail if another thread holding the *lock* panicked. in this case, we call unwrap() have this thread panic. 

* *Mutex<T>* is a smart pointer called *MutexGuard* (*lock* returns it).

	* impl *deref*, and *drop* (releases the lock automatically).


### Sharing a Mutex<T> Between multiple threads. 



```rust, editable

use std::sync::Mutex;
use std::thread; 


fn main() {

	let count = Mutex::new(10);

	let mut handles = vec![];

	for _ in 0..10 {
		
		let handle = thread::spawn(move || {
			
			let mut num = count.lock().unwrap();
			*num += 10;
		
		});

		handles.push(handle);

	};

	for h in handles {

		h.join().unwrap();

	};

	println!("count : {:?}", count);
}

```


* this code does not compile. using mutex<T> in multiple thread is not allowed and also Rc<T> type can not use in this case as well because Rc<T> is not safe across threads. 



### Atomic reference counting with Arc<T> 

* atomics work like primitive types but are safe to share across threads. 

	* it can cause performance panalty.


```rust, editable

use std::sync::{Mutex, Arc};
use std::thread; 


fn main() {

	let count = Arc::new(Mutex::new(10));

	let mut handles = vec![];

	for _ in 0..10 {
		
		let count = Arc::clone(&count);
		
		let handle = thread::spawn(move || {

			let mut num = count.lock().unwrap();
			
			*num += 10;
		
		});

		handles.push(handle);

	};

	for h in handles {

		h.join().unwrap();

	};

	println!("count : {:?}", count);
}

```

### Similarities between Refcell<T>, Rc<T> and Mutex<T>, Arc<T> 

* *mutex* also provide interior mutablility. 

* using Rc<T> come with the risk of creating reference cycles and also Mutex<T> come with the risk of creating *deadlocks*.



### extensible concurrency with the Sync and Send Traits. 

* the *Send* marker trait indicates that ownership of the type implementing *Send* can be transfered between threads. 

	* almost every Rust type is *Send*, but there are some exceptions, including Rc<T>: 
	if you tried to tranfer it across thread, both threads need to be updated count.

	* almost all primitive types are *Send*, aside from raw pointers. 


### allowing acess from Multiple threads with Sync. 

* the *Sync* makers trait indicates that it is safe for the type implementing *Sync* to be referenced from multiple threads. In other words, any type T is *Sync* if &T(a reference to T) is *Send*, meaning the reference can be send safely to another thread. 

* the smart pointer Rc<T> is also not Sync for the same reasons that it's not Send. 

	* RefCell and Cell types are not **Sync**. 

		* Cell is used for scalar type that implements Copy trait, otherwise Refcell is used more comflex type. 


### Implementing Send and Sync Manually is Unsafe. 

* thoes are *Maker traits*, they do not have methods to implement. 

* we will talk about unsafe code in chapter 19 and building new concurrent type without those need to be careful thought. 


### summary

* handler, join : block thread until it finished.

* channel (mpsc) : recv(), try_recv() 

* mutex, arc 

* send and sync as maker type. 


