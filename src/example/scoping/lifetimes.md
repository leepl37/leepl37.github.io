# Lifetimes


### part 1 라이프타임
 러스트에서는 borrow checker가 존재. 이 borrow check는 borrow한 값을 보장한다.
 여기서 보장한다는 말은 라이프타임의 시작과 끝을 보장한다는 말이다.

 예제를 통해 살펴보자.
```rust

 fn main () {

 let i = 3; // 라이프 타임 i 시작.

 {
     let borrow1 = &i; // 'borrow1'  라이프타임 시작.

     println!("borrow1 : {}, 라이프 타임 끝 ", borrow1);
 }
 {
     let borrow2 = &i; // 'borrow1'  라이프타임 시작.

     println!("borrow2 : {}, 라이프 타임 끝 ", borrow2);
 }

 }  // 라이프타임 i 끝.
```



### part 2 Explicit annotation, 명확한 선언

 참조한 값에 라이프타임 선언이 필요하다.
 생략이 가능한 부분이 많은데, 생략이 불가능한 부분에서 사용한다.

```rust
 foo<'a>
// foo 는 라이프타임 파라미터 `'a` 를 갖는다.
```

 * 클로져에서 라이프타임을 사용할 때 제네릭과 동일한 방법으로 표기한다.
 * foo<'a> 에서 foo는 `'a`  보다 라이프타임이 짧다.

```rust

 fn main() {
   
   fn fn_life_time_longer<'a>() {
         let t = 3;

         // let lifetime_valie: &'a i32= &t;
         // t 는 함수에 지정된 라이프타임보다 더 일찍 드랍되기 때문에 사용이 불가능하다.
     }
 }
```

### part3 Functions

 * 함수에서 사용된 참조값은 지정된 라이프타임 값이 존재해야 한다. 단 생략 가능한 경우는 제외.
 * 함수에서 사용된 참조값 중에 리턴될 참조 값은 input 된 파라미터와 동일한 라이프 타임 참조값을 가지고 있어야 하거나 스태틱이여야 한다.
 * input 없이 참조값을 리턴하는 것은 있을 수 없는 일이다.( 참조할 값이 없기 때문)

```rust
 fn main() {
	// 동일한 라이프타임 소유
     fn return_lifetime_str_after_compare<'a>(a : &'a str, b : &'a str) -> &'a str {
         if a.len() > b.len() {
            return a
         }
         b
     }

     let a = "hi";
     let b = "tony";
     let result = return_lifetime_str_after_compare(a, b);

     println!("{}", result);

 }
```

### part4 Bounds 


 * `T: 'a` : All references in `T` must outlive life time `a. 
 * `T: Trait + 'a ` : Type T must implement trait `Trait` and all references in `T` must outlive 'a.


```rust

 use std::fmt::Debug;
 fn main() {
     #[derive(Debug)]
     struct Ref<'a, T: 'a>(&'a T);

     fn print<T>(t: T)
     where
         T: Debug,
     {
         println!("print which impl Debug trait : {:?}", t);
     }

     fn print_ref<'a, T>(t: &'a T)
     where
         T: Debug + 'a,
     {
         println!(" print wich impl Debug and 'a : {:?}", t);
     }

     let st = "tony";
     let t = Ref(&st);

     print_ref(&t);
     print(&t);
 }
```


### part 5 Coercion 

* A longer lifetime can be coerced into a shorter one. 

```rust 

fn multiply<'a>(first: &'a i32, second: &'a i32) -> i32 {
     first * second
}

// 'a 는 'b 와 동일한 라이프타임 이라는 의미 -> 'b 로 output 를 내기 때문에
// first 를 return 하든 second 를 return 하든 관계 없음
fn choose_first<'a: 'b, 'b>(first: &'a i32, second: &'b i32) -> &'b i32 {
     first
}

fn main() {
     let a = 3i32;
     let b = 6i32;

     println!("{}", multiply(&a, &b));

     println!("{}", choose_first(&a, &b));
 }

```


### part 6 Static 

* reserved name `'static` 

```rust
 let s: &'static str = "hello tony";

 fn generic<T>(x: T) where T: 'static {}
 ```

 위의 두 코드의 차이점을 알아보자.

####  Reference lifetime 
 `'static` 이라고 표시된 데이터는 프로그램이 실행될 동안 항상 존재한다. 물론 coercion 규칙도 적용되어 shorter 라이프타임으로도 쓰일 수 있다.  
변수를 `'static` 라이프타임으로 만들 수 있는 방법이 두 가지가 존재한다. 그리고 이 데이터들은 read-only memory of the binary 이다. 

 * Make a constant 변수에 `static` 선언하기. 
 * Make a `string` literal -> `&'static str` type 


 ```rust

 static NUM:i32 = 18;

 fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
     &NUM 
 }
 ```

#### Trait bound 

트레이트 바운드에서 `T: 'static` 의 뜻은 `static` 이 아닌 레퍼런스는 허용하지 않는다는 뜻이다.  
즉 특정 데이터에서 참조값을 준 데이터는 허용되지 않고 `static` 이거 owned data 여야 한다. 
```rust

use std::fmt::Debug;

fn print_it(input : impl Debug + 'static ) {

	println!("static value : {:?}", input );

}

```

### part 7 Elision 
 라이프타임 룰은 꽤나 규칙적이고 자주 볼 수 있기 때문에 borrow checker 에서 생략 가능 하도록
 허용한다. 이를 elision 이라 한다. 
 * first - 모든 래퍼런스 파라미터에 라이프타임이 추가된다. 
 * second - 만약 input 파라미터가 하나라면 output 값에도 동일한 라이프타임 파라미터가 적용된다. 
 * third - 만약 multiple input 라이프타임 파라미터를 갖고 그 중 하나가 `&self` 또는 `&mut self`
 라면 `self`의 라이프타임이 output 라이프타임 파라미터에 적용된다. 





