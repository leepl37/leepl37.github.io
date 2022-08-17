# Ownership, Move, Borrowing



### part 1 오너쉽 그리고 이동 (Ownership and moves)

메모리에 저장된 리소스는 오직 한 변수만을 오너로 삼는다. 참고할 것은 모든 변수가 리소스를 소유하고 있는 것은 아니다. 레퍼런스를 가질 때도 있기 때문이다. 

`let x = y`를 선언하거나 foo라는 함수에 `foo(x)` 를 적용하였을 시에는 오너쉽이 이동하게 된다. 
y는 x 변수 안으로, 함수에서 x는 foo라는 함수 안으로 이동.


러스트에서는 이러한 이동을 `move`라고 얘기한다. 

이렇게 이동하게 된 값, 즉 y나 함수에서 사용된 x값은 더 이상 사용할 수 없다. 
이렇게 하면 dangling pointers 를 예방할 수 있다. 


### part 2 변환 (Mutability)

오너쉽이 다른 변수로 이동하였을 때, 변환타입(immutable -> mutable) 변경이 가능하다. 

```rust

 let immut_box = Box::new(5u32);

 let mut mut_box = immut_box;

```

### part 3 부분 이동(Partial moves)

변수를 destructuring 할 때, `by-move`, `by-reference` 패턴 바인딩이 동시에 가능하다. 
다만 이렇게 할 때, 부분적으로 변수가 이동하게 된다. 즉 변수의 한 부분이 이동할 때 다른 한 부분은 변수에 머물게 된다. 
이런 경우는 해당 변수는 전체로써 사용은 불가능하지만 부분적으로는 사용이 가능하다. 


```rust,editable

fn main() {


     #[derive(Debug)]
     struct Person {
         name : String,
         age : Box<u8>
     }
      
     let person = Person { name: String::from("Tony"), age: Box::new(35) };
    
     let Person { name, ref age } = person;
   
     println!("what is your name : {:?}", name);
     let name_move = name;
    
     println!("{:?}", name_move);
    
     println!("{:?}", person.age);
     // println!("{:?}", person); // 전체 객체로는 사용이 불가능
}

```


### part 4 빌림 (Borrowing)

대부분 데이터에 접근할 때 오너쉽을 가져와서 사용하지 않는다. 이때 러스트에서는 빌림 메커니즘을 이용한다(borrowing mechanism). 
값을 (`T`) 를 그대로 던져주는 것이 아닌 (`&T`) 를 사용한다.

러스트 컴파일러는 정적인 상태에서도 (borrow checker 를 통해) 우리가 사용 중인  reference 에 대해 항상 값이 존재하는 것을 보장한다. 
즉 참조값이 존재한다는 것은 해당 객체가 없어지는 않는다는 것이다. 


```rust,editable

fn main() {


     fn eat_box_i32(boxed_i32 : Box<i32>) {
         println!("Destroying box that contains {}", boxed_i32);
     }
   
     fn borrow_i32(borrowed_i32 : &i32) {
         println!("This int is : {}", borrowed_i32);
     }
   
     let boxed_i32 = Box::new(5_i32);
     let stacked_i32 = 6_i32;
     
     borrow_i32(&boxed_i32);
     borrow_i32(&stacked_i32);
   
     {
         let _ref_to_i32 = &boxed_i32;
         println!("{:?}", _ref_to_i32);
     } 
     // &boxed_i32 값은 _ref_to_i32 로 이동하였지만 {} 스쿠핑 룰에 의해 참조값은 드랍된다. 
     // 온전한 값으로 다시 사용 가능.
    eat_box_i32(boxed_i32);
   }

```


### part 5 변환 (Mutability)
변환 데이터 (Mutable data) 는 `&mut T` 를 이용하여 mutably borrowed 가 가능하다. 이를 *mutable reference* 라 하며 참조값에 대해 읽기/쓰기 접근이 가능하다. 반대로 `&T` 는 immutably borrowed 가 가능하며 *immutable reference* 하며 참조값에 대해 읽기 접근만 가능하다. 


```rust,editable


fn main() {
  
    struct Book {
        author : &'static str,
        title : &'static str,
        year : u32,
    }

    fn borrow_book(book : &Book) {
        println!("immutably borrowed {} - {} edition", book.title, book.year);
    }

    fn new_edition(book : &mut Book) {
        book.title = "Rust란";
        book.year = 2014;
        println!("mutably borrowed {} - {} edition", book.title, book.year);
    }

    let mut immutabook = Book {
        author: "tony",
        title: "rust란",
        year: 2020,
    };


    borrow_book(&immutabook);

    new_edition(&mut immutabook);

}

```




























