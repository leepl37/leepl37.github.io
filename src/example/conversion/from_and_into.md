# From and Into

From 과 Into 매우 유용하며 개념만 이해한다면 쉽게 사용할 수 있다. 

간단하게 설명하고 예제로 알아보자. 
먼저 A 라는 enum 또는 struct 이 존재한다고 하자. 
A 는 B를  가져와서 A 화 할 수 있다.

마찬가지로 B 도 원하면 A 가 될 수 있다. 말이 이상한데 예제로 살펴보자.
앞에서 종종 나왔지만 String::from 과 같은 trait 를 구현한 것이다.

```rust,editable

#[derive(Debug)]
struct Gwano {
    kind: String, 
}


// std::lib 에 구현된 from tait 를 이용하여 gwano Struct 을 구현하였다. 
impl From<String> for Gwano {
    fn from(value : String) -> Self {
        Gwano {
            kind : value,
        }
    }
}


fn main() {
   
   let k = Gwano::from("good".to_string()); 

   println!("{:?}", k);
   

   //into 는 from 을 구현하면 자동으로 구현된다고 생각하자. 
   //주의해야할 점은 into() 함수 사용 시, 타입을 필수로 지정해야 한다. 그래야 컴파일러가 어떤 타입으로 converting 할지 
   //알 수 있다.

   let kk = "veryGood".to_string();
   let answer:Gwano = kk.into();
   
   println!("{:?}", answer);
}


```


# TryFrom and TryInto 

from and into 와 비슷한데, 리턴 값이 Err 일 수 있는 상황에서 유용하다. Result<> 타입은 추후에 자세히 다룬다. 
이런 것이 있다 정도만 알아두자.
```rust,editable

#[derive(Debug)]
struct Gwano {
    kind: String, 
}

// 구현된 tryform trait 을 이용해 gwano 를 impl 한다. 
impl TryFrom<String> for Gwano {
    type Error = ();

    fn try_from(value: String) -> Result<Self, Self::Error> {
        if value == "answer".to_string() {
            let answer = Gwano {
                kind : value ,
            };
            Ok(answer)
        } else { 
           Err(()) 
        }
    }
}


fn main() {
    
    let check = "answer".to_string();
    let n_check = "not".to_string();

    let an1 = Gwano::try_from(check);
    
    println!("{:?}", an1);
    let an2 = Gwano::try_from(n_check);
    println!("{:?}", an2);
}

```


