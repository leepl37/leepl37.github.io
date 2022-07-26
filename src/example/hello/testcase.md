# TestCase: List(iter 개념과 Option 개념이 필요함)

그렇다면 배열이 있는 `struct` 일 경우, `fmt::Display` 로는 어떤 식으로 구현할까?

```rust,editable
  use std::fmt::Display;


  #[derive(Debug)]
  struct ListVec(Vec<i32>);

  impl ListVec {
      fn new() -> Self {
          ListVec(vec![1,2,3,4,5,6,7])
      }
  }

  impl Display for ListVec {

      // iter() 는 for 문의 형태로 쓰인다는 점에 알아두자.
      // 리턴 타입이 Result 인데, 이는 ? 를 사용할 수 있다는 의미이다. 
      // 간단히 눈으로 익히고 뒤에 자세히 알아보자.

      fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
	  
          write!(f, "list start ");     
          for (count, n) in self.0.iter().enumerate() { 
              if count != (self.0.len() - 1) {
	      	write!(f, "{} -> ", n)?;
	      }else{
	      	write!(f, "{}", n);
	      }
          }

          write!(f, " finished.")
      }
  }

  fn main(){
      println!("{}", ListVec::new());

      println!("{:?}", ListVec::new());

  }

```
