# Modules 모듈

모듈이란 function, structs, traits, `impl` 블록 또는 다른 모듈 을 포함한 영역을 설명한다.코드의 영역을 나누거나 할 때 쓰인다. 

### Visibility

* `mod` - module 영역 선언
* `super` - 상위 단계에서 사용할 수 있도록 선언
* `private` - default 

러스트에서 default 는 private 이며 pub 선언 시 다른 영역에서 사용이 가능하다. 예제를 통해 알아보자.

```rust,editable

pub mod my_mod {
    use self::nested::nested_fn_only_called_by_super_mod;

  
    pub fn test() {
        println!("hello tony good morning");
    }

    fn private_fn() {
        println!("it is private but can called by same module fn")
    }

    pub fn will_call_private_fn() {
        private_fn();
    }

    mod nested {
  
        fn nested_fn() {
            println!("nested fn");
        }

        pub(super) fn nested_fn_only_called_by_super_mod() {
            nested_fn();
        }

    }

    pub fn can_call_nested_fn_can_nested_private_fn() {
        nested_fn_only_called_by_super_mod();
    }

}

fn main() {
    my_mod::test();
    my_mod::will_call_private_fn();
    my_mod::can_call_nested_fn_can_nested_private_fn();
}

```
mod 이라고 선언하는 것이 하나의 파일을 만들어 그 안에 작성하는 것과 거의 동일한 역할을 한다. ex)
test.rs 라는 파일 안에 pub hello fn 을 구현하고 현재 파일 내에서 mod test; 하면 해당
모듈(파일내함수) 들을 import 하여 쓰는 것과 동일한다.




### Struct Visibility and use keyword

다른 언어에서와 비슷하게 동작한다. 크게 어렵지 않게 이해할 수 있다.

* `mod` - module 선언
* `associated fn` - new function 
* `use`, `crate`, `as` - Alias 사용가능


```rust, editable

mod my {
    pub struct OpenBox<T> {
        pub contents : T,
    }

    pub struct ClosedBox<T> {
        contents: T,
    }

    impl<T> ClosedBox<T> {
        pub fn new(contents: T) -> Self {
            ClosedBox { contents: contents }
        }
    }
}

use crate::my::ClosedBox as CBox;
fn main() {
    let open_box = my::OpenBox { contents: "can put anything, it is public" };
   
    // let closed_box = my::ClosedBox { contents: "can't put anything, it is private" };
    let closed_box = my::ClosedBox::new("struct can only be created when you use new associated fuction.");

    let t = CBox::new("I will call this CBox");

    println!("{:?}", t);
}


```


### super and self 

* super and self 개념은 자바스크립트나 자바에서 말하는 this == self, super 는 부모 모듈을 말한다. 
