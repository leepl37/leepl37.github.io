# Scoping rules(오너쉽, 참조, 라이프타임)


Scoping rules - ownership 과 borrowing 그리고 lifetimes 에서 중요한 역할을 한다. 


### part 1 - RAII (Resource Acquistion Is Initialization)
러스트에서 변수는 데이터를 스택에 저장하는 것만 하는 것은 아니다. --> 리소스를 소유하고 있다. 
예로 Box<T> 는 힙에 저장된 메모리를 소유하고 있다. 
러스트는 RAII 를 강요하는데, 특정 객체가 스코프를 벗어나게 되면, 이 객체는 destructor 를
호출하고 소유하고 있던 리소스 또한 없어진다. 

이러한 행위는 리소스 누수 버그를 예방하고 수동적으로 메모리를 없앨 필요가 없다. 
예제를 통해 살펴보자.


```rust
fn create_box() {
    // Allocate an integer on the heap, 힙에 지정 i32 type 
    let _box1 = Box::new(3i32); 

    // `_box1` is destroyed here, and memory gets freed, scope 가 끝나는 시점에서 메모리에서 없어짐. 
}


fn main() {

    // Allocate an integer on the heap, 힙에 i32 타입 지정
    let _box2 = Box::new(5i32);


    // A nested scope:  scope 만들기. 
    {
        // Allocate an integer on the heap, scope 안에서 힙에 i32 타입 지정
        let _box3 = Box::new(4i32);

        // `_box3` is destroyed here, and memory gets freed, scope 끝나는 시점에서 destructor 호출 -> drop 
    }

    // Creating lots of boxes just for fun
    // There's no need to manually free memory!,

    //1000 개를 힙에 저장시키지만 단 한 개도 남지 않는다. 
    
    for _ in 0u32..1_000 {
        create_box();
    }

    // `_box2` is destroyed here. 


}

```


### part 2 Destructor 

스코프를 벗어날 때 호출되는 destructor 는 러스트가 제공하는 `Drop` trait 에 의해 실행된다.
자동으로 실행되며 따로 코드를 impl 할 필요는 없지만 필요하다면 구현할 수도 있다. 
변수가 scope 를 벗어나려는 순간, destructor 는 호출되며 drop 이 실행된다. 



```rust

    // part 2 
    struct ToDrop;
    impl Drop for ToDrop {
        fn drop(&mut self) {
            println!("이 객체는 dropped 된다.");
        }
    }
    fn main() {

    let x = ToDrop;
    println!("곧 드랍될 예정");

}


```


