# pointers/ref
 
포인터와 참조 개념을 알아보자. 

일단 매치에서 쓰이는 포인터와 참조 개념에 대해서 알아보자.

* 참조된 값을 reference, & 로 표현하며 반대로 참조된 값을 참조에서 벗길때는 *을 사용하며 Derefencing 이라고 한다.

* 매치에서 Destructuring 할 떄에는 &, ref 와 ref mut 을 사용한다. 

추후 다른 단원에서 이러한 개념에 대해 깊게 다루니 그때 이해하면 되니까 너무 이해하려고 하지말자.

단박에 이해가 되면 좋지만 그렇지 못함으로 반복과 집중을 통해 이해를 높이자.

```rust,editable

fn main() {
    let ref_val = &3;

    // 여기서 ref_val 는 &3 이다.
    match ref_val {
        &v => println!("& 를 이용한 match"),
    }

    // &3 -> 3 으로 매치  
    match *ref_val {
        v => println!("value 를 dereferencing 함.")
    }

    
    let not_ref_val = 3;
    
    // 선언 시 변수 앞에 ref 를 추가하여 &변수로 구현할 수 있다.
    let ref change_ref_val = not_ref_val;

    let value = 3;
    let mut mut_value = 3;
    

    match value {
        // value -> &val 로 출력할 수 있다. 
        ref r => println!("Create a ref val : {}", r),
    }

    //mutable 한 value를 ref mut 로 구현 후 dereferencing 하여 변환
    match mut_value {
         ref mut m => {
            *m += 10;
            println!("ref mut val : {}", m)
        }
    }

}

```


