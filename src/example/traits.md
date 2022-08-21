# Traits


### part1 Traits

* `trait` 는 함수의 집합이라고 볼 수 있다. 또한 선언된 함수들은 함수들 끼리 서로 호출할 수 있다.

* 선언된 함수의 집합들은 어떤 데이터 타입에서도 implement 할 수 있다.


### part2 Derive

* 러스트 컴파일러는 `#[derive] attribute` 라는 선언을 이용해서 trait 을 이용할 수 있다.
* 또한 직접 `#[derive]` 말고 `trait` 을 직접 구현할 수 있다.

종류

* 비교 traits : `Eq`, `PartialEq`, `Ord`, `PartialOrd`
* `Clone`, T 라는 객체를 &T 객체를 통해 Copy.
*  `Copy`, Move 대신에 해당 type 을 Copy 한다.
*  `Hash`, to compute a hash from &T.
*  `Default`, to create an empty instance of a data type.
*  `Debug`, to format a value using the `{:?}` formatter.

### part2 Returning Traits with dyn
함수에서 `return` 타입을 지정할 때에는 명확한 타입이 필요하다. 그렇지 않으면 컴파일 타임에 `return` 타입에 사이즈를 알 수 없기 때문이다. 그렇다면 `trait` 타입은 어떻게 리턴할 수 있는가? 박스 타입을 이용하면 가능하다. 박스 타입은 레퍼런스이며 힙에 저장된 메모리이다. 레퍼런스는 사이즈를 알 수 있기 때문에 박스로 감싸면 `return` 이 가능하다.

```rust,editable

use std::fmt::Debug;
trait Human {
    fn get_name(&self) -> &str;
 }

#[derive(Debug)]
struct Tony {
     name: String,
 }

 #[derive(Debug)]
 struct Hee {
     name: String,
 }

 impl Human for Tony {
     fn get_name(&self) -> &str {
         &self.name
     }
 }

 impl Human for Hee {
     fn get_name(&self) -> &str {
         &self.name
     }
 }

 fn get_struct_name<T>(n: T) -> Box<dyn Human>
 where
     T: Human,
 {
     if n.get_name() == "gwano" {
         Box::new(Hee {
             name: "hee".to_string(),
         })
     } else {
         Box::new(Tony {
             name: "gwano".to_string(),
         })
     }
 }


 fn main() {
     let t = Tony {
         name: "gwano".to_string(),
     };
     let h = Hee {
         name: "hee".to_string(),
     };

     println!("{:?}", get_struct_name(t).get_name());
     println!("{:?}", get_struct_name(h).get_name());
 }

```


### part3 Operator Overloading


`연산자` 는 `trait` 을 통해 Overloading이 가능하다.
```rust,editable
use std::ops::Add;

#[derive(Debug)]
struct Bar;
#[derive(Debug)]
struct Foo;
#[derive(Debug)]
struct FooBar;
#[derive(Debug)]
struct BarFoo;

impl Add<Foo> for Bar {
     type Output = BarFoo;

     fn add(self, rhs: Foo) -> BarFoo {
         BarFoo
     }
 }

 impl Add<Bar> for Foo {
     type Output = FooBar;

     fn add(self, rhs: Bar) -> Self::Output {
         FooBar
     }
 }
 fn main() {
     let t = Bar.add(Foo);

     let tt = Foo + Bar;
     println!("{:?}", tt);
     println!("{:?}", t);
 }
```

### part4 Drop

`Drop` trait 는 `drop` 함수만을 가지고 있으며 해당 객체가 `scope` 에서 벗어날 때 자동으로
실행되는 함수이다. `Drop` 을 사용하는 이유는 해당 자원을 Free 시키기 위함이다.
`Box`, `Vec`, `String`, `File`, and `Process` 타입들이 대표적으로 `Drop Trait` 을 implement 한 것이다. 또한 `Drop trait` 은 수동적으로도 implement 하는 것이 가능하다.

```rust,editable

struct Droppable {
     name : &'static str,
 }

 impl Droppable {
     fn new(name : &'static str) -> Self  {
         Droppable { name }
     }
 }

 impl Drop for Droppable {
     fn drop(&mut self) {
        println!("will drop this name of struct : {:?}", self.name);
     }
 }


 fn main() {
     let a = Droppable::new("A");

     {
         let b = Droppable::new("B");
         {
             let c = Droppable::new("C");
         }
     }

     drop(a); // 직접 드랍하기.
 }
```


### part5 Iterators

iter Trait 를 구현하기 위해서는 `next` 요소만 구현하면 된다.
또는 arrays 나 ranges 의 경우는 자동으로 구현됨.

```rust,editable

struct Fibonacci {
     curr: u32,
     next: u32,
 }

impl Iterator for Fibonacci {
     type Item = u32;

     fn next(&mut self) -> Option<Self::Item> {
         let new_next = self.curr + self.next;

         self.curr = self.next;
         self.next = new_next;

         Some(self.curr)
     }
 }

 fn fibonacci() -> Fibonacci {
     Fibonacci { curr: 0, next: 1 }
 }

 fn main() {
     for  i in fibonacci().take(4) {
         println!("> {}", i);
     }

 }
```


### part6 impl Trait

 `impl Trait` 은 보통 아래 두 가지 방법으로 쓰일 수 있다.

* 파라미터
* 리턴 타입

#### 파라미터 타입으로 쓰일 경우



```rust,editable

fn parse_csv_documnet<R: std::io::BufRead>(src : R) {}

fn parse_csv_documnet_using_impl(src : impl std::io::BufRead) {}
```

#### 리턴 타입으로 쓰일 경우

```rust,editable

use std::iter;
use std::vec::IntoIter;

// This function combines two `Vec<i32>` and returns an iterator over it.
// Look how complicated its return type is!
 fn combine_vecs_explicit_return_type(
     v: Vec<i32>,
     u: Vec<i32>,
 ) -> iter::Cycle<iter::Chain<IntoIter<i32>, IntoIter<i32>>> {
     v.into_iter().chain(u.into_iter()).cycle()
 }

// This is the exact same function, but its return type uses `impl Trait`.
// Look how much simpler it is!
 fn combine_vecs(
     v: Vec<i32>,
     u: Vec<i32>,
 ) -> impl Iterator<Item=i32> {
     v.into_iter().chain(u.into_iter()).cycle()
 }

// Returns a function that adds `y` to its input
// clouse 는 이름이 없는 콘크리트 타입을 가진다. impl Fn(타입) 으로 지정.
 fn make_adder_function(y: i32) -> impl Fn(i32) -> i32 {
     let closure = move |x: i32| { x + y };
     closure
 }

//you can also use impl Trait to return an iterator that uses map or filter closures. This makes
//using map and filter easier. Because closure types don't have names, you can't write out an
//explicit return type if your function returns iterators with closures. But with impl Trait you
//can do this easily

 fn double_positives<'a>(numbers: &'a Vec<i32>) -> impl Iterator<Item = i32> + 'a {
     numbers
         .iter()
         .filter(|x| x > &&0)
         .map(|x| x * 2)
 }

 fn main() {
     let plus_one = make_adder_function(1);
     assert_eq!(plus_one(2), 3);

     let singles = vec![-3, -2, 2, 3];
     let doubles = double_positives(&singles);
     assert_eq!(doubles.collect::<Vec<i32>>(), vec![4, 6]);
 }
```


### part6 Supertraits

러스트에는 "inheritance" 가 존재하지 않지만 `trait` 에 또 다른 `trait` 을 구현할 수 있다.

```rust,edtiable


 trait Person {
     fn name(&self) -> String;
 }

 trait Student: Person {
     fn university(&self) -> String;
 }

 trait Programmer {
     fn fav_language(&self) -> String;
 }

 trait ComSciStudent: Programmer + Student {
     fn git_username(&self) -> String;
 }

 struct A;

 impl Person for A {
     fn name(&self) -> String {
         "gwano".to_string()
     }
 }

 impl Student for A {
     fn university(&self) -> String {
         "kyunghee_uni".to_string()
     }
 }

 impl Programmer for A {
     fn fav_language(&self) -> String {
         "rust".to_string()
     }
 }

 impl ComSciStudent for A {
     fn git_username(&self) -> String {
         "leepl37".to_string()
     }
 }

 fn comp_sci_student_greeting(student: &dyn ComSciStudent) -> String {
     format!(
         "My name is {} and I attend {}. My favorite language is {}. My Git username is {}",
         student.name(),
         student.university(),
         student.fav_language(),
         student.git_username()
     )
 }

 fn main() {
     
     let who = comp_sci_student_greeting(&A);
     println!("{}", who);
 }

```


### part7 Disambiguating overlapping traits 
만일 두 개의 `trait` 을 impl 하였는데 동일한 이름이 존재한다면 어떻게 할 것인가?
`impl` 블락이 다르기 때문에 구현 시에는 문제가 없지만 해당 객체를 호출할 때 문제가 된다.
이럴 땐 full syntax 을 사용한다. 


```rust,editable

trait UsernameWidget {
    fn get(&self) -> String;
}

trait AgeWidget {
    fn get(&self) -> u8;
}

struct Form {
    username : String,
    age : u8,
}

impl UsernameWidget for Form {

    fn get(&self) -> String {
        self.username.clone()
    }
}

impl AgeWidget for Form {
    fn get(&self) -> u8 {
       self.age 
    }
}


fn main() {
    
    let form: Form = Form {
        username: "rustacean".to_owned(),
        age: 35,
    };
    
    let t = UsernameWidget::get(&form);

}

```








