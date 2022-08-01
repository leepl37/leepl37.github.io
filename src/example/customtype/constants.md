# constants

러스트에는 두 종류의 불 변수가 존재한다. 

* `const` : 불변하는 value(주로 사용)
* `static` : static 이라는 라이프 타임을 지정하며 `mut`able 하게 변수 지정할 수 있다. mutable static 변수에 접근하거나 변경하는 것은
`unsafe` 를 사용한다.



```rust



// constants 타입은 어디든 선언이 가능하다.
static LANGUAGE : &str = "러스트"; // 변수명은 항상 대문자
const THRESHOLD : i32 = 10;

fn main() {
    
    println!("{LANGUAGE}");

    println!("{THRESHOLD}");
   
    // 불변수에는 재선언 불가능
    // THRESHOULD = 5; 
    // LANGUAGE = "rust"; 

}


```

`static` 개념에 대해서는 추후에 자세하게 다룰 예정이다. 눈으로 이런 문법이 있다 정도면
익히고 넘어가면 된다. 

개인적인 생각으로는 러스트는 다른 언어에 비해 러닝커브가 까다롭다. 여러번 훑어본다는 느낌으로 전체를 이해하고 이해한 후에 코드를 짜다보면 조금씩 나아진다. Rust! 
