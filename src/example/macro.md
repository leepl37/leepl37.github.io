# macro

### part 1 macro_rules!

러스트에 매크로 시스템은 메타프로그래밍이 가능하다. 함수와 비슷하지만 끝에 `!` (bang) 이 붙는다.

```rust

fn main() {
     
     // macro_rules! 매크로이름 {
     //  생성된 함수 => statement
     // }

     macro_rules! say_hi {
         () => {
             println!("hello");
         };
     }

     say_hi!();
 }
```


### part2 Designators

* 매크로의 인수는 `$` 로 표현한다.
Designator 종류
`block`, `expr`, `ident`, `item`, `literal`(literal constants), `pat`(pattern), `path`, `stmt`(statement), `tt`(token tree), `ty`(type), `vis`(visibility qualifier)

```rust

fn main() {
     macro_rules! create_fn {
         // ident 라는 designator 를 사용하고 함수 $fn_name 을 생성한다.
         ($fn_name : ident) => {
             fn $fn_name() {
                 println!("I make fn name {:?}", stringify!($fn_name));
             }
         };
     }

     create_fn!(gwano);

     gwano();


     macro_rules! print_result {
         // expr 라는 designator 를 사용
         ($expression:expr) => {
             println!("{:?} = {:?}",
                     stringify!($expression),
                     $expression);
         };
     }

     print_result!(2 + 2);

 }
```


### part3 Overload
매크로는 다른 조합의 인수도 오버로드를 통해 받아들일 수 있다.
`match` 와 비슷하게 동작한다.

```rust

macro_rules! test {
     ($left:expr; and $right:expr) => {
         println!("{:?} and {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left && $right) // bool
     };
     // 새로운 단락에서 (arm) 세미콜론을 붙여줘야 한다.
     ($left:expr; or $right:expr) => {
         println!("{:?} or {:??} is {:?}",
                  stringify!($left),
                  stringify!($right),
                  $left || $right) // bool
     }
 }

 fn main() {
     test!(1i32 + 1 == 2; and 2i32 + 0 == 2); // bool type만 비교 가능
     test!(5i32 + 5 == 10; and 10i32 + 5 == 15); //bool type만 비교 가능
 }


```

### part4 Repeat
 * `+` 인수로 사용하는데, 이는 적어도 한번은 반복하겠다는 의미이다.
 * `*` 아예 반복을 하지 않거나 여러 번 하겠다는 뜻.
 * `$(...), +` 로 감싸진 인수는 이에 맞는 단락에 매치되고 `,` 로 다음 인수를 분류한다.

```rust

 macro_rules! find_min {
     ($x:expr) => ($x); // A Case

     ($x:expr, $($y:expr),+) => (  // B, C Case

         std::cmp::min($x, find_min!($($y),+))
         )
     }

 fn main() {
         println!("{}", find_min!(1)); // A Case

         println!("{}", find_min!(1 + 2, 2)); // B Case
         // 3, 2 => min(3, 2) => 2
         println!("{}", find_min!(5, 2 * 3, 4)); // C Case
         // 5, (6, 4) => min(6, 4) => 4 => min(5, 4) => 4
 }


```

### part5 Don't Repeat Yourself

```rust

use std::ops::{Add, Mul, Sub};

 macro_rules! assert_equal_len {
     // The `tt` (token tree) designator is used for
     // operators and tokens.
     ($a:expr, $b:expr, $func:ident, $op:tt) => {
         assert!($a.len() == $b.len(),
                 "{:?}: dimension mismatch: {:?} {:?} {:?}",
                stringify!($func),
                 ($a.len(),),
                 stringify!($op),
                 ($b.len(),));
     };
 }

 macro_rules! op {
     ($func:ident, $bound:ident, $op:tt, $method:ident) => {
         fn $func<T: $bound<T, Output=T> + Copy>(xs: &mut Vec<T>, ys: &Vec<T>) {
             assert_equal_len!(xs, ys, $func, $op);

             for (x, y) in xs.iter_mut().zip(ys.iter()) {
                 *x = $bound::$method(*x, *y);
                 // *x = x.$method(*y);
             }
         }
     };
 }

 // Implement `add_assign`, `mul_assign`, and `sub_assign` functions.
 op!(add_assign, Add, +=, add);
 op!(mul_assign, Mul, *=, mul);
 op!(sub_assign, Sub, -=, sub);

 mod test {
     use std::iter;
     macro_rules! test {
         ($func:ident, $x:expr, $y:expr, $z:expr) => {
             #[test]
             fn $func() {
                 for size in 0usize..10 {
                     let mut x: Vec<_> = iter::repeat($x).take(size).collect();
                     let y: Vec<_> = iter::repeat($y).take(size).collect();
                     let z: Vec<_> = iter::repeat($z).take(size).collect();

                     super::$func(&mut x, &y);

                     assert_eq!(x, z);
                 }
             }
         };
     }

     // Test `add_assign`, `mul_assign`, and `sub_assign`.
     test!(add_assign, 1u32, 2u32, 3u32);
     test!(mul_assign, 2u32, 3u32, 6u32);
     test!(sub_assign, 3u32, 2u32, 1u32);
 }
```


### part 6 Domain Specific Languages(DSLs)
 
러스트에 속해 있는 embedded 된 macro 언어라고 생각하자. 

```rust


 macro_rules! cal {
     (eval $e:expr) => {
         {
             let val: usize = $e; // 타입을 integer 로 제한한다. 
             println!("{} = {}", stringify!{$e}, val);
         }
     };
 }


 fn main(){

     cal! {
         eval 1 + 2
     }

     cal! {
         eval 1 + 3 
     }
 }


```



































