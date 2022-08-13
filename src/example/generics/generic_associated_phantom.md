# Associated items & Phantom type parameters

말 그대로 Associated item 을 제네릭 타입과 결합하여 어떻게 활용할 수 있는지 알아보자.

trait 에서도 다른 제네릭 타입을 받을 수 있으며 이를 활용할 수 있는데, 
아래 예제에서 처럼 제네릭을 선언할 수 있다. 하지만 이렇게 선언해버리면 코드 짜기가 여간 불편한 게 아니다. 

```rust
trait Contains<A, B> {
    ....
}


```

 해당 trait 를 impl 할 떄 예제이다.

 ```rust
             // 타입 A, B를 추가해야함 
impl Contains<i32, i32> for SomeThing {


}

	// 해당 trait 을 이용한 함수를 선언할 때에도 A, B, C 타입에 대해서도 
	// 선언해야 한다.  
fn diff<A, B, C>(con : &C) -> i32 
	where C : Contains<A, B>  {
	
	}

 ```

이러한 문제를 해결해보자.
바로 trait 안에 타입을 선언하는 것이다. 예제를 통해 알아보자.


```initrust,editable

	// 해당 trait 안에 A, B 타입 선언
trait Contains {
    type A;
    type B;
    
   	// 함수에 위에서 선언한 타입을 이용 
    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;

    fn first(&self)-> i32;
    
    fn last(&self) -> i32;
}

struct Container(i32, i32);

	// 구현할 struct 내부에서 타입 선언
impl Contains for Container {
    type A = i32;
    type B = i32;

    fn contains(&self, a: &Self::A, b: &Self::B) -> bool {
        self.0 == *a && self.1 == *b
    }

    fn first(&self)-> i32 {
        self.0
    }

    fn last(&self) -> i32 {
        self.1
    }
}

	// 함수 선언 시에도 하나의 타입만 선언하면 된다.
fn difference<C>(con : &C) -> i32 
    where C : Contains {
        con.last() - con.first()
    }



fn main() {
    let t = 3;
    let tt = 3;

    let c = Container(3, 3);
    let result = c.contains(&t, &tt);
    println!("{:?}", result);

    println!("{:?}", difference(&c));

}

```

기존에는 A = B = C 가 같은 영역 안의 타입으로 지정되었다면,  
Associated type 을 활용하면 C 영역 안에 A와 B 타입이 선언된다. 
당연히 C type 을 구현할(impl) 타입에서 A와 B을 선언하니 A, B 타입을 따로 선언할 필요가
없게 된다.






