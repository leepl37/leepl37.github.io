# Custom Types

커스텀 타입에는 `struct` 와 `enum` 그리고 Constants가 있다.


### Structrues

`struct` 키워드로 생성할 수 있다. 

* Tuple structs, 안에 튜플을 가진 객체이다.
* The classic C structs. C언어 스타일의 객체
* Unit structs 필드는 존재하지 않는다. Generic 제니릭 부분에서 유용하게 쓰인다.

```rust

//튜플 struct
struct Pair(i32, f32);

struct Point {
	x: i32,
	y: f32,
}

struct Unit;


```


### Enums

`enum` 키워드로 생성할 수 있다.

*선언된 enum 안에 필드는 struct과 동일하게 여러 타입들이 선언될 수 있다.
*선언된 enum 안에 필드는 C 언어 처럼 0, 1, 2 인덱싱이 가능하다.


type aliases

`enum` 명이 길다면 aliases 선언 가능.

ex)

```rust
enum verylongverboseEnum {
	A,
	B
}

type short = verylongverboseEnum;

fn main(){
	let x = short::A;
}
```
