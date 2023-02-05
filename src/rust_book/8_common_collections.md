# COMMON COLLECTIONS, Ch 8

unlike the built-in array and tuple types these collections point to is stored on the heap memory.

which is that the size is not determined at compile time. 

it means these collections can shrink and grow as the program rus. 

#### we'll cover..

create and uipdate vectors, strings, and hashmaps as well as what makes each special. 

* A vector - allows you to store a variable number of values next to each other. 

* A string - is a collection of characters. 

* A hashmap - allows you to associate a value with a particular key. it's a particular implementation of the more general data structure called a map. 


### About Vectors. 

* vector can only store values of the same type. 

* create vector, we can use Vec::new() from std lib or vec!["value"] macro. 
	* when we create vector, we need to specify what the type of vector. 
		* but some cases compiler might infer the type if we add value after create. 

* get value from vector, two methods are represent..
	* vec.get(index) - return type is Option type -> no compile time error always return Option.  
	* vec[index] - return type is value of that vector. -> compile error occured if it does not have value. 

* can not use mutable vector after referencing to other value.
	* because adding a new elements onto the end of the vector might require allocating new memory and copying the old elements to the new space. so this might result in pointing to a deallocated memory. 


### Different types that can store in vector. 

#### vector can only take value of same type so we can not use struct type. Instead, enum that can be useful. But this type can not be added at runtime. what should we do?  -> trait object, cover in chapter 17. 



### About Strings. 

#### strings in rust are implemented as a collection of bytes, plus some methods to provide useful functionality when those bytes are interpreted as text. 

we'll discuss.. 
 
* indexing into a string and why it is difficult, what is differences between how people and computers interpret string data. 

* string slices, which are references to some UTF-8 encoded string data stored elsewhere. 
	* string literals, for example, are stored in the program's binary and are therefore string slices. 

* String type, which is provided by std lib is a growable, mutable, owned, UTF-8 encoded string type. 
	* std lib also include OsString, OsStr, CString, and CStr. 

* create String, String::new(); or "value".to_string() method, this method need to be the type that implemented the Display trait, as string literals do. 
	* to_string method is same as String::from("value"). 



### updating string. 

* s.push_str("value"), took ownership of str value. 

* s.push('c'), adding char. 

* add(self, &str) -> s1 + &s2.  coerce the &string argument into a &str. 
	* deref coercion which is &string into &string[..] 


* using a format! macro. format!("{} - {}", s1, s2);

### Indexing into Strings.

* A String is a wrapper over a Vec<u8>

* depends on Unicode scalar value in string, each char use different byte size. 

* char() or bytes() 


### Hash Maps 

The type HashMap<K, V>

* store their data on the heap. 

* all the keys must have the same type, and all of the values must have the same type. 

* use zip to create hash map. ex) teams = vec!["blue","red"], scores = vec![10, 20] -> teams.iter().zip(scores.iter()).collect(); 

* hashmap take ownership. 

* only inserting a value if the key has no value. ex) entry("blue").or_insert(50); 

* updating a value based on the old value. ex) entry("blue") returns value of key. and if value is not exist, insert value. 


### Hasing Functions 

hashmap uses a cryptographically strong hasing function that can provide resistance to Denial of Service (Dos) attacks. This is not the fastest hashing algorithm available, but the trade-off for better security that comes with the drop in performance is worth it. 







