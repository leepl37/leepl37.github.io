# As input parameters


그렇다면 `Closure` 을 input parameter 로 받을 때에는 타입 지정을 어떻게 해야할까?
`std::lib` 에 지정된 trait 을 이용하는데 3가지 trait 을 이용하여 타입을 지정한다.

* `Fn` - &T -> 오직 참조
* `FnMut` - &mut T -> 참조 그리고 변환
* `FnOnce` - T -> 참조 변환 그리고 이동


예제를 통해 알아보자.

```rust,editable

fn use_fn<F>(fn_: F)
where
    F: Fn(),
{
    fn_()
}

fn use_fn_mut<F>(fn_mut: &mut F)
where
    F: FnMut(),
{
    fn_mut()
}

fn use_fn_once<F>(fn_once: F)
where
    F: FnOnce(),
{
    fn_once()
}

fn main() {
    

    //Fn 을 사용한 경우
    let x = "just using this value as ref";
    let fn_closure = || {
        println!("Fn : use fn as parameter : {x}");
    };

    use_fn(fn_closure);

    //println!("Fn : still can use this closure {:?}", fn_closure());
    

    //FnMut 사용한 경우

    let mut y = "using fn mut and see how it works".to_string();

    let mut fnmut_closure = || {
        println!("FnMut : the sentence was -- {}", y);
        y = "it works".to_string();
        println!("FnMut : the sentence changed -- {}", y);
    };

    use_fn_mut(&mut fnmut_closure);

    //println!("FnMut : still can use this closure {:?}", fnmut_closure());

    //fnonce 사용한 경우
    
    let z = "using this value as FnOnce".to_string();

    let fnonce_closure = || {
        println!("FnOnce : let's using this value as fnonce.");
        //두가지 방법으로 이동 시킬 수 있는데, 직접 변수에 캡쳐링 한 값을 넣는 경우
        // let move_this_value_to = z;
        //std::lib 에 있는 drop 함수를 이용해서 메모리에서 드랍 시키는 경우
        drop(z);
    };

    use_fn_once(fnonce_closure);
    //아래 주석을 풀면 컴파일 에러가 난다.
    // println!("{z}");
}

```
