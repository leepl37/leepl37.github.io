# As output parameters


인풋 타입으로 클로져를 파라미터로 사용하는 것이 가능한 것 처럼 아웃풋 파라미터로도 사용이 가능하다.
하지만 클로져 자체가 익명함수이기 때문에 반환 값으로 타입을 지정하기가 애매하다. 
이럴땐 trait 로 리턴 값을 지정하면 된다.

`Fn`, `FnMut`, `FnOnce` 가 `trait` 로 구현된 것이기 때문이다. 추후에 `trait` 챕터에서 자세하게 다룰 예정이니 
눈으로 익히고 넘어가시길 바랍니다.

각 예제 마다 `move` 키워드를 사용했는데 왜 사용했을까?

`move`와 `참조` 개념과 관련이 있는데, 블록 안에서는 변수 사용이 가능하지만 블록 밖으로 나가면 해당 변수는 메모리에서 드랍된다. (드랍에 대해서도 추후 챕터에서 나온다.) 
드랍된 변수는 더 이상 사용이 불가능하기 때문에 해당 `closure`는 변수 사용이 불가능하기에 컴파일 에러가 난다.



```rust,editable

fn create_fn() -> impl Fn() {
    let fn_text = "fn_text".to_string();

    move || println!("this is a : {fn_text}")
}

fn create_fnmut() -> impl FnMut() {
    let fnmut_text = "fnmut_text".to_string();

    move || println!("this is a : {fnmut_text}")
}

fn create_fnonce() -> impl FnOnce() {
    let fnonce_text = "fnonce_text".to_string();

    move || println!("this is a {fnonce_text}")
}

fn main() {
	// return 값이 클로져이다. 클로져를 실행하자. 
    let fn_function = create_fn();
    fn_function();

    let mut fnmut_function = create_fnmut();
    fnmut_function();

    let fnonce_function = create_fnonce();
    fnonce_function();
}
```
