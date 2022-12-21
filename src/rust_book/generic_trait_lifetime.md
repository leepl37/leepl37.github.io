# Generics Types, Traits, and Lifetimes


generic과 trait 그리고 lifetimes 을 한번에 묶어서 설명하는데 이유는 간단하다.

trait 바운드라는 제약을 걸어 generic 타입의 행위를 제한할 수 있다.

이때 lifetime 은 실은 generic 타입으로 선언할 수 있으며 선언된 lifetime은 각 각의 값과 관계되어

lifetime을 표시한다고 볼 수 있다.


먼저 제네릭 타입의 이용과 선언 방법에 대해 알아보자.

```rs

fn main() {
  println!("hello");
}


```
