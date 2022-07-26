# Debug & Display

러스트 내에 `std` library 에 포함된 타입들은 자동으로 프린트 매서드가 구현되어있다. 
이외의 다른 타입들은 *무조건* 구현해야만 프린트 할 수 있다. 

구현할 수 있는 방법은 기본적으로는 두가지가 존재한다.

바로 Debug 와 Display 이다.


### Debug

그 중 Debug에 대해 먼저 알아보자.

`fmt::Debug trait`은 굉장히 사용자 편의성이 좋은 편이다. `derive` 를 선언하면 자동으로 `fmt::Debug` 를 구현한다. 
ex) 자바의 애너테이션 개념과 비슷하다고 표현해도 무방할듯..

예제와 같이 알아보자.

참고 - Debug 사용 시, 프린트 매크로에는 항상 {:?} 또는 {:#?} 을 사용하셔야 합니다. 

```rust
fn main() {


// 기본 타입들은 프린트가 가능.
let t = "hello_world";
let tt = 3; 

println!("프린트 == {t}, {tt}  == 가능");

//사용자 지정타입, 즉 std library 에서 제공되지 않음 -> 프린트 불가.
struct CanNotPrint(i32);

//아래 코드는 주석을 풀면 에러가 납니다.
//println!("{:?}", CanNotPrint(3));


//Debug 선언
#[derive(Debug)]
struct CanPrintWithDebug(i32);

println!("{:?}", CanPrintWithDebug(3));


#[derive(Debug)]
struct Person<'a> {
	name: &'a str,
	age:u8
}

let tony = Person { name : "과노", age : 34 };

//{:?}, {:#?} 비교 
println!("{:?}", tony);
println!("{:#?}", tony);


}

```


### Display

Display 에 대해 알아보자.

`Debug` 가 `derive` 를 통해 자동으로 프린트를 구현하였다면 `Display` 는 조금 더 손이 많이 간다. 즉 사용자가 직접 구현해야한다는 말이다. 다시 말하면 프린트하는 포맷을 내 입맛에 맛에 손 볼 수 있다. 

예제를 통해 알아보자.


```rust,editable
  use std::fmt::Display;

  struct PrintWithDisplay(i32);

  impl Display for PrintWithDisplay {
      // Display 를 구현함에 따라 Display 에 존재하는 default 함수를 가져와서 이를 구현한다.

      /*
       *다소 이해가 가지 부분들이 있으리라 생각됩니다만, 앞으로 나올 내용이니 너무 고민마시>  고
       * 추후 배울 내용을 미리 눈으로 익힌다는 느낌으로 보시는 게 좋을 듯 합니다.
       */

      fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
          write!(f, "내가 원하는 입맛대로 출력하기. {}", self.0)
      }
  }

  fn main() {
      let t = PrintWithDisplay(3);     
      // 단순히 {} 만 사용하여도 출력가능 -> Display 구현했기 때문.
      println!("{}", t);
  }
```



### 비교

```rust,editable
  use std::fmt::Display;


  #[derive(Debug)]
  struct DebugAndDisplay {
      x: String,
      y: String,
  }


  impl Display for DebugAndDisplay {
      fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
          write!(f, "내 기준에서 y가 먼저지! {} 그리고 {}", self.y, self.x)
      }
  }



  fn main() {
      let t = DebugAndDisplay{ x: "다른 프로그래밍 언어...".to_string(), y: "러스트".to_string() }; 

      println!("Debug  : {:?}", t);
      println!("Display : {}", t);
  }

```

### 참고

`Vec<T>` 타입과 같은 경우는 `Display` 를 구현할 수가 없는데, `제네릭`의 경우는 어떠한 포맷으로 구현할지 타입마다 다르기 때문에 애매하다. 따라서 `Vec<T>` 는 `Display` 로 구현되지 않는다는 점을 알아두자. 



