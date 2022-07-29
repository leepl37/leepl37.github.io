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

다른 예제를 살펴보자.

```rust

  use std::fmt::Display;

  struct City {
       
       // &'static 에 대해서 후반에 다룰 예정이니 String 포인터 개념이지만 스택에 올라간
       // 데이터라고 생각하고 넘어가자.

      name : &'static str,
      lat : f32,
      lon : f32
  }

  impl Display for City {

      // 여기서 `f` 는 버퍼인데 간단하게 부하를 줄이기 위해 잠시 데이터를 저장하는 공간
      // String 포맷으로 구현하되 이를 버퍼에 저장한다고 생각하자.
      fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {

          let location_of_latitude = if self.lat > 0.0 { 'N' } else { 'S' }; 

          let location_of_longitude = if self.lon > 0.0 { 'E' } else { 'W' };

          write!(f, "location of {} : {:.3}, {}, {:.3}, {}",
                 self.name, self.lat, location_of_latitude,
                 self.lon, location_of_longitude
                 )
      }
  }



  fn main() {

      let t = City{ name: "Vancouver", lat: 49.25, lon: -123.1 };
      println!("{t}");

  }



