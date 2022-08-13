# Attributes, 속성

모듈, 크래이트, 아이템 등에 적용되는 것을 말하는데 이 속성에는 metadata 가 들어있어 그 속성이 적용된다.

* 컴파일할 때 조건 적용
* 크래이트 이름, 버전, 타입 setting
* warnings 표시 유무
* macros, glob imports 적용
* 외부 라이브러리와 연결
* 함수에 unit 테스트라고 표시


예제)
* crate 적용 시 - #![crate_attribute], bash script 처럼 샤뱅 적용. 
* 모듈이나 아이템에 적용 시 -  #[item_attribute] 

```txt

#[attribute(value, value2)]

#[attribute(value, value2, value3, value4)]

```


### dead_code

러스트 코드를 짤 때 unused function 에 대해 warnings 을 나타내는데 꽤나 귀찮을 때도 있다.
이때 dead_code attribute 를 사용하면 표시되지 않는다.

```txt

#[allow(dead_code)]
fn unused_function() {}

```

책에서 말하기를 사용되지 않는 코드는 아예 지우는 편을 추천한다. 물론 동의하는 바이다. 같이 일하는 사수가
귀에 딱지가 앉도록 코드 정리가 중요하다고 얘기하고 있는데 감사하게 생각한다.... 많이 배우고 있다.... 
thank to Min 부장님..



### cfg 

Configuration 의 약자이며  OS 체크 시 유용하다.

* cfg 는 attribute.
* cfg! macro 


```rust,editable
//  attribute를 이용한 os 체크
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
	println!("yes. running on linux!");
}
// attribute 
#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
	println1(" Not running on linux!");
}


fn main() {

	are_you_on_linux();
	// 매크로를 사용하여 체크
	if cfg!(target_os = "linux") {
		println!(" yes yes. It is linux");
	}

}

```

### Custom 

target_os 와 같은 몇몇은 rustc 에 의해 제공된다. 하지만 입맛대로 생성하여 조건을 맞출 수 있다.

```rust

#[cfg(custom_my_condition)]
fn my_custom_condition() {
	println!(" condition met! ");
}


fn main() {
	my_custom_condition();
}

```

`cargo r` 하면 컴파일 에러가 나온다. 해당 컨디션을 컴파일러에게 미리 말해주어야 한다.

```shell
$ rustc --cfg custom_my_condition main.rs ** ./main 

```

해당 컨디션은 컴파일에게 알려지고 컴파일 후 해당 함수가 실행된다.











