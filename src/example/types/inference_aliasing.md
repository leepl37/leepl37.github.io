# inference_and_aliasing

### Inference 타입 추론

러스트에 존재하는 타입 추론 엔진은 특별한 타입이나 상황을 제외하고는 직접 타입을 지정하지 않아도 된다.

* 변수에 값을 선언하였을 시 추론 가능
* 변수 선언 후 추후에 값을 지정해도 추론 가능


```rust,editable

fn main() {
	
	let elem = 5; //변수에 값 지정, 타입 추론 가능
	

	let mut vec = Vec::new(); // Array 계열인 빈 벡터 선언, 컴파일러는 아직 타입을 알지 못함. Vec<_> 타입인 상태

	vec.push(elem);
	
	// vec의 타입은 Vec<i32> 가 된다. 러스트는 자연수 default 는 i32 이기 때문이다.
	println!("{:?}", vec);
	

	//여기서 알아둬야 할 부분은 선언 후 초기화를 하지 않으면 컴파일 에러가 난다. 컴파일 타임에 메모리의 양을 알 수 없거니와
	//타입 지정 추론이 아예 불가능하기 떄문이다. 
	//만약 변수 선언만 하고 초기화는 하지 않고 싶다 또는 추후에 하고 싶다면 변수 선언 시 타입을 지정하여야 한다. 
	let typed_vec::Vec<i32>;  
	let typed_num::i32; 

	
}

```


### Aliasing 

linux를 조금이라도 써본 사용자라면 alias 가 얼마나 유용한지 알 수 있다. 러스트에서도 비슷한 맥락으로 사용된다. 
타입에 대한 alias 선언을 알아보자. - 추후 많은 타입에 대해서도 alias 가 가능한데, 개념만 알면 자연스럽게 익히게 된다.

```rust,editable

type NanoSecond = u64; // u64는 NanoSecond 라는 타입으로 사용할 수 있다.
type Inch = u64; // 러스트에서 Alias 는 UpperCamelCase 를 활용한다.

fn main() {
	
	let nanosec : NanoSecond = 5;

}

```
