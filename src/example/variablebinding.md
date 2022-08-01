# Variable Bindings,변수 바인딩

러스트는 static typing 을 통한 type safety 제공한다. 타입은 변수 선언시 지정할 수 있지만
대부분 컴파일러에 의해 타입이 선언될 수 있다. 


### Mutability 

러스트는 기본으로 불변수인데 mut 를 사용하면 수정이 가능하다.

```rust,editable

fn main() {
	

	let last_name = "lee";
	// 불가능
	// last_name = "kim";
	
	//mut
	let mut name = "hello";
	name = "world";
	println!("{name}");
}
```
### Scope and Shadowing 
러스트에서 Scope 영역은 해당 변수의 드랍 여부를 나타내는 중요한 지표이다. 
(드랍이라는 것은 더 이상 참조가 불가능하며 바로는 아니지만 메모리에 제거 된다는 말이다.)


```rust,editable
fn main() {
	
	// Scope

	let scope = 1;
	{
	 let inner_scope = 2;

	 println!("{inner_scope}");

	}
	// 이 블록 영역이 지나면 호출 불가능, 드랍된다.
	//println!("{inner_scope}");
	
	println!("{scope}");
	

	//Shadowing

	let shadow = 1;
	
	{
		let shadow = "shadow";
		println!("{shadow}");
	}
	// 같은 이름으로 선언하였지만 드랍되어 쓰일 수 없다.
	println!("{shadow}");
}
```

 
### Declare first, 변수 선언과 초기화

러스트는 변수를 선언하고 그 후에 초기화 하는 것이 가능하다. 그러나 초기화하지 않고 사용하는 것은 컴파일 에러를 나타낸다.

```rust,editable

fn main() {
	//선언
	let declare;
	{
		let a = 2;
		declare = a * a;

	}	
	//위에 초기화값은 블록 안에서도 가능한다. 왜냐하면 변수는 상위에 선언되고 블록 안에서는 상위에 선언된 변수가 value 값만 들어가기 때문이다. (Move 개념_ 추후 설명) 
	println!("{declare}");
	
	//선언 후 초기화 작업 없이는 사용 불가능
	//let a_declare;
	//println!("{a_declare}");
}

```



### Freezing 

데이터가 mut 하게 선언되었다가 다시 immut 로 선언되면 이것을 freez 라고 한다. 
더 이상 mutable 하게 사용이 불가능하기 때문이다.

```rust,editable


fn main() {
	let mut number = 3;

	{
	// 해당 변수에 value 값만 들어간다. 쉐도잉으로 새롭게 선언된 변수는 immut 하다.
	let number = number;
	
	//그렇기에 아래와 같은 구문은 불가능하다.
	//number = 1; 

	}
	// 위의 문장들은 블록 영역이 끝이 나면서 드랍되기 때문에 더 이상 쓸모 없게 되며

	// 맨 위 mut number 변수에 영향을 받는다.
	number = 6;

	println!("{number}");
}

```








