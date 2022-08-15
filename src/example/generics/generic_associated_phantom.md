# Associated items & Phantom type parameters

말 그대로 Associated item 을 제네릭 타입과 결합하여 어떻게 활용할 수 있는지 알아보자.

trait 에서도 다른 제네릭 타입을 받을 수 있으며 이를 활용할 수 있는데, 
아래 예제에서 처럼 제네릭을 선언할 수 있다. 하지만 이렇게 선언해버리면 코드 짜기가 여간 불편한 게 아니다. 


### Associated items


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




### Phantom type 

팬텀 타입은 런타임에는 나타나지 않고 컴파일 타임에 나타나 체크하는 타입을 말합니다.
데이터 타입은 제네릭 타입을 파라미터로 활용하여 표시용이나 컴파일 타입에 체크용으로 활용할 수 있습니다.
이러한 타입은 메모리에 어떠한 데이터도 갖고 있지 않으며 런타임에도 아무 영향을 미치지 못합니다.

```rust, editable

use std::marker::PhantomData;

#[derive(PartialEq)]
struct PhantomTuple<A, B>(A, PhantomData<B>);

#[derive(PartialEq)]
struct PhantomStruct<A, B> {
    first: A,
    phantom: PhantomData<B>,
}

// 메모리에 'A' 타입은 지정되어 있지만 'B'는 지정되어 있지 않다.

fn main() {
    let _tuple1: PhantomTuple<char, f32> = PhantomTuple('Q', PhantomData);

    let _tuple2: PhantomTuple<char, f64> = PhantomTuple('Q', PhantomData);

    println!(" tuple 1 == tuple 2 can not be compared because they are different type");

    let _struct1: PhantomStruct<char, f32> = PhantomStruct {
        first: 'S',
        phantom: PhantomData,
    };

    let _struct2: PhantomStruct<char, f64> = PhantomStruct {
        first: 'S',
        phantom: PhantomData,
    };

    // println!(" struct 1 == struct 2 비교할 수가 없다. 이 둘의 타입이 다르기 때문 : {}", _struct1 == _struct2);
}


```


### TestCase : Unit clarification 

팬텀타입에 관해 활용 케이스를 알아보자.

[영상으로 배우기](https://www.youtube.com/watch?v=Qr9PhKURf2g)

```rust,editable
use std::ops::Add;
use std::marker::PhantomData;


// Add 함수에서 객체가 카피되기 때문에 Clone과 Copy를 구현하였다.
#[derive(Debug, Clone, Copy)]
enum Inch {}

#[derive(Debug, Clone, Copy)]
enum Mm {}



// Length는 Unit 을 제네릭으로 받는 Struct이다. 
// 여기서 팬텀 타입은 타입 체크를 위해 추가한 것.
#[derive(Debug, Clone, Copy)]
struct Length<Unit>(f64, PhantomData<Unit>);


impl<Unit> Add for Length<Unit> {
    type Output = Length<Unit>;

	// length Struct 에서 첫번째 값만 연산을 하고 두번째 팬텀데이터는 타입 체크를 위해 활용되기 때문에, PhantomData 라고 추가하여 리턴한다.
    fn add(self, rhs: Length<Unit>) -> Length<Unit> {
        // '+' 를 사용하면  Add 함수를 호출한다. 
        Length(self.0 + rhs.0, PhantomData)
    }

}


fn main() {
	    
	    // 동일한 타입만 연산됨. 
    let inch_1: Length<Inch> = Length(3.0, PhantomData);

    let return_inch = inch_1 + Length(12.0, PhantomData);

    println!("{:?}", return_inch);

    let mm_1 : Length<Mm> = Length(1000.0, PhantomData); 
    let mm_2 : Length<Mm> = Length(1000.0, PhantomData); 

    
    let return_mm = mm_1 + mm_2; 

    println!("{:?}", return_mm);


}

```
































