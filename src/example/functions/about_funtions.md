# Functions

* 러스트에서 function,함수는 `fn` 키워드로 선언하며 함수 안에 변수는 타입 지정이 필수이다.
* 함수에서 리턴 값이 있으면 `->` 로 표시하며 그 뒤에 타입을 입력한다. 
* 함수 안에 리턴 값은 구문이 아닌 식으로 표현해야 하기 때문에 `;`이 없어야 한다. (앞선 챕터 expression 참고)
* 만약 `if` 구문으로 인하여 아래 구문들이 실행되기 전에 `return` 해야 한다면 `return` 뒤에 `리턴값`을 입력하면 된다.



```rust,editable
//함수 선언
fn make_fn(num1 : i32, num2 : i32) -> i32 {
	num1 * num2  //식, return value 
}


fn no_return_fn(num1 : i32, num2 : i32) -> () {
	println!("no return fn, 여기선 -> () 입력하였지만 생략 가능. "); // 구문 
}

fn return_value_at_num1(num1 : i32) -> i32 {
	if num1 == 1 {
	 	return num1 * 100
	}else{
		num1 * 0
	}

}

fn main() {
	let num1 = 1;
	let num2 = 3;

	println!("make_fn : {}", make_fn(num1, num2));

	no_return_fn(num1, num2);

	println!("return_value_at_num1 : {}", return_value_at_num1(num1));

}
```
