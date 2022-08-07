# match

러스트에서는 C `switch` 와 비슷한 개념으로 패턴 매치를 제공하는데 기본적인 테크닉을 익히면 아주 유용하게 사용할 수 있다.

간단히 설명하면 value 값을 매칭 시켜 분기를 태울 수 있다.

```rust

fn main() {
	let say = "hello";
	// _ 의미는 else 개념으로 이해하면 된다.
	match say {
		"hello" => println!("he said {}", say),
		_ => println!("nothing")
	}
}

```

match Destructure 에 대해 알아보자.


### Destructuring 

예제는 tuple 로만 표현했지만 struct, enum, array 으로도 destructure 가능하다.

```rust,editable

fn main() {
	
	let tuple_match = (0, 1, 2);
	
	match tuple_match {

		(0, y, z) => println!("first is 0, then random y: {}, z :{}" ,y ,z),
		(1, ..) => println!("first is 1, then ..."),
		_ => println!("I dont care what first number is .."),
	}
}

```


