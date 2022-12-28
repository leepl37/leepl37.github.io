# Generics Types, Traits, and Lifetimes


generic과 trait 그리고 lifetimes 을 한번에 묶어서 설명하는데 이유는 간단하다.

trait 바운드라는 제약을 걸어 generic 타입의 행위를 제한할 수 있다.

이때 lifetime 은 실은 generic 타입으로 선언할 수 있으며 선언된 lifetime은 각 각의 값과 관계되어

lifetime을 표시한다고 볼 수 있다.


### 제네릭 타입 선언 방법

* 먼저 제네릭 타입으로 struct, enum, function, method  선언하는 방법을 알아보자.


### Struct 

* struct의 변수 값을 generic타입으로 선언하고자 한다. 즉 해당 변수는 i8, i16, i32, String, str, Vec.. 등 여러 타입을 가질 수 있는 변수로 만들고자 한다는 의미이다.

```rs

 struct Point<T> {
 	x : T,
	y : T,
}

```

* 선언은 struct name 뒤에 제네릭 타입을 임의로 설정한다. 이때 보통 대문자를 사용하며 제네릭을 사용한다는 표시이기도 하다. 

* 염두하여야 될 것은 point struct 에 선언한 T 타입은 x와 y 변수에 동일하게 지정되었다. 즉 x와 y는 동일한 타입이여야 한다는 뜻이다.

Ex)
- let same_type = Point { x : 3, y : 3 };  —> complie 시 문제 없음 

- let different_type = Point { x : 3, y : 3.0 }; -> compile 에러 발생

```rs 

Struct Point<T, U> {
	x : T, 
	y : U,
}

```

Ex) 
위와 같이 generic 타입을 T, U 를 선언하고 x와 y 변수에 각각 T와 U를 선언한다. 
이렇게 선언 시에는 위에서 발생한 컴파일 타입에러는 발생하지 않는다. 

let can_have_a_different_type = Point { x : 3, y : 3.0 };  —> complie 시 문제 없음

Enum : generic enum 타입 사용 

평시에 자주 사용하는 Option과 Result에 대한 것도 제네릭으로 구현되어 있음을 확인할 수 있다. 

아래의 코드들은 prelude 로 사용할 수 있는 generic 사용한 enum 타입이다. 
```rs

Enum Option<T> {
	Some(T),
	None,
}

Enum Result<T, E> {
	Ok(T),
	Err(E),
}

```



```rs 

use std::fmt::Display;

struct Point<T, U> {
    x: T,
    y: U,
}

impl Point<i32, f32> {
    fn get_default_location(&self) -> (i32, f32) {
        (self.x, self.y)
    }
}

// 특정 제네릭 타입의 struct 이지만 concrete type 으로 지정하여 선언할 수도 있다.
// 또한 이렇게 선언된 struct 는 i32 타입일 경우에만 함수 호출이 가능하다.

impl<T, U> Point<T, U> {
    fn use_x_and_get_the_oher_y<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

// Impl 뒤에 T, U 를 선언하는 것은 해당 Point struct 가 제네릭 타입이라는 것을 미리 알 수 있어야 하기에 선언이 필요하다. 
// 또한 이 함수는 다른 Point struct 을 파라미터 값으로 받으며 해당 point 는 다른 타입을 가진다.
// 리턴 값은 Point Struct 가 되며 이때 해당 point struct 에 x값은 해당 함수를 호출하는 point에 x,  y값은 파라미터 값으로 받은 point의 y 값으로 만든다.
// 해당 함수는 파라미터 point 의 타입을 선언해야 되기에 함수명 바로 뒤에 <> 로 제네릭 타입을 선언하여 사용한다.


impl<T, U> Display for Point<T, U> where T : Display, U : Display {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "x is : {}, y is : {}", self.x, self.y)
    }
}



fn main() {
    let p1 = Point {
        x: 3,
        y: 3.0,
    };

    let p2 = Point {
        x: String::from("A"),
        y: String::from("B"),
    };

    println!("{:?}",p1.get_default_location());
    println!("{}", p1.use_x_and_get_the_oher_y(p2));

}


```

### 성능 문제.
Enum Option<T> {
	Some (T),
	None,
}

Let t = Some(3);

Let t =  Some(3.0);

-> Compile 

Enum Option<i32> {
	Some(i32),
	None,
}

Enum Option<f32> {
	Some(f32),
	None,
}


모노몰피즘  컴파일 될 때 마다 해당 타입으로 코드가 만들어진다. 때문에 성능에 문제는 없음
Rust’s generics extremely efficient at runtime !

Trait defining shared behavior.

트레잇은 특정 언어의 인터페이스와 유사하다.  공통으로 사용할 함수를 선언 후에 트레잇을 사용할 struct 이나 enum, trait 에 implements를 하여 사용할 수 있다. 

Trait Summary {
	fn summarize(&self) -> String; 
}

trait을 선언하고 뒤에 이름을 지정한 후 원하는 함수들을 선언한다. 이때 선언하는 함수의 바디 부분은 정하지 않고 
Implements 하는 쪽에서 바디를 구현하면 된다. 


Struct Tweet {
	username : String,
	content : String,
	reply : bool,
	retweet : bool
}

Impl Summary for Tweet {
	fn summarize(&self) -> String {
		println!(“tweet ! {}”, self.username);
		self.username
	}
}

위의 예제는 tweet struct 에 summary trait 을 implements 한 것이다.  Summary trait 에서 사용했던 함수 이름을 그대로 사용하되 함수의 바디를 구현하였다. 

이렇게 구현하게 되면 tweet instance 는 summary trait 의 summarize 함수를 호출할 수 있으며 tweet instance 에 해당되는 함수 바디가 존재하게 되는 것이다.

유용한 예제)

Struct NewsArticle {
	headline : String,
	location : String,
	author : String,
	content : String,
}

Impl Summary for NewsArticle {
	fn summarize(&self) -> String {
		println!(“newArticle! {}”, self.headline);
		self.author
	}

}


 Fn who_has_been_written_this<T : Summary >(from : T)  {
		from.summarize();
}


둘다 가능.

 Fn who_has_been_written_this(from : impl Summary)  {
		from.summarize();
}
위와 같이 함수에 파라미터 타입을 summary trait 타입으로 한 후에 이용할 수 있다. 



Default implementations 

Trait Summary {

	fn summarize(&self) -> String {

		format!(“(Read more from {} … )”, self.summarize_author())
	}

	fn summarize_author(&self) -> String;

}



Default 함수를 선언할 수 있는데 trait 에 바디 부분을 미리 선언해 둘수있다.  이때 다른 함수를 호출하는 방식으로 이용할 수 있다.


알아둘 것.
이를 구현한 쪽에서 바디를 구현한 default 함수도 오버라이드 할 수 있다. 


