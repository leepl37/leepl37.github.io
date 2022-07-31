# Primitives, 원시타입

러스트 언어는 타입 지정이 우선시 되어야 한다. 항상 이를 염두하자.

### Scalar Types 
* signed integers: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
	
* unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128` and `uisze`(pointer size)

* floating point: `f32`, `f64`

* `char` Unicode 인 `'a'`, `'α'` and `'∞'` (4 bytes each) 

* `bool` either `true` or `false`

* and unit type `()`, empty typle 이라고 하는데 아래에서 알아보자.


튜플 타입 임에도 불구하고 컴파운드 타입으로 고려되지 않는 것은 하나의 타입으로 지정되어 있기 때문이다.

### Compound Types

* arrays like `[1,2,3]`
	
* tuples like `(1, true)`



타입 지정할 때 숫자는 suffix 로 지정할 수 있다.
숫자는 default 타입이 존재하는데 Int 는 i32, float는 f64 가 있다.
또한 러스트는 타입 infer 시스템을 가지고 있다.

```rust, editable

fn main() {

      let _logical: bool = true;

      let _a_float: f64 = 1.0; // Regular annotation

      let _suffix_integer = 3i32; // Suffix annotation i32

      let _default_float = 3.0; // default f64

      let _default_integer = 7; // default i32                                                               
      //주석을 풀면 에러남.
      //println!("{default_integer}");

      // 러스트는 기본적으로 변수가 immutable 이기 때문에 mut 를 변수 앞에 선언할 수 있다.

      let mut can_change = "나는 예전엔 어리석었다.".to_string(); 

      println!("{can_change}");

      can_change = "현재는 무지에서 벗어났다".to_string();

      println!("{can_change}");


      //아래 코드는 주석을 풀면 에러가 난다. 같은 타입에서만 변화만 가능하다.
      //사람이 제 아무리 노력해서 변화한다 한들 다람쥐나 고래가 될 수 없듯이
      //can_change = 3;

      }

```

# Tuples 

튜플 타입은 () 으로 지정한다. 
(type1, type2, type3...) 과 같은 방식으로 선언할 수 있다.

```rust, editable

  fn reverse_tuple( pair : (i32, bool) ) -> (bool, i32) {
      let (reverse_bool, reverse_i32) = pair; i32, bool
      (reverse_i32, reverse_bool)
  }


  fn main() {

     let t = (33, true); (i32, bool)
     let result_t = reverse_tuple(t); (bool, i32)

     println!("{:?}", result_t);

  }

```









