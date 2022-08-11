# Section 1


### Results and Options 

this section, we are going to learn about Results and Options in rust.

Before using Results and Options in rust std lib, we are going to make our own result.

```rust,editable
//lets build our own result type.

#[derive(Debug)]
enum Res<T, E> {
    Thing(T),
    Error(E),
}

//우리가 만든 Res, result 타입은 T, E 제네릭 타입으로 어떤 타입으로도 생성이 가능하다.
//아래 함수에서 쓰일 T, E 의 타입은 i32, String 으로 정한다.
fn divide(a: i32, b: i32) -> Res<i32, String> {
    if b == 0 {
        // b 가 0 이면 divide 가 불가능함으로 바로 return 한다.
        return Res::Error("0으로 나누는 것은 불가능합니다.".to_string());
    }
    Res::Thing(a / b)
}

fn main() {
    let a = divide(3, 0);

    if let Res::Error(e) = a {
        println!("error : {e}");
    }
}


```


```rust
pub struct Stepper {
    curr: i32,
    step: i32,
    max: i32,
}

impl Stepper {
    fn new() -> Self {
        Stepper {
            curr: 1,
            step: 3,
            max: 55,
        }
    }
}
impl Iterator for Stepper {
    // 현재 iter 를 구현한 stepper 의 item type 은 i32 이며
    type Item = i32;

    // next 함수 호출 시, max 값 보다 적을 때만 값을 리턴한다.
    fn next(&mut self) -> Option<Self::Item> {
        if self.max < self.curr {
            return None;
        }
        // 현재 값을 리턴하고 curr 값에는 step 만큼 증가 시킨다.
        let res = self.curr;
        self.curr += self.step;
        Some(res)
    }
}

fn main() {
    // associate fn 을 이용하여 객체를 생성
    let mut step = Stepper::new();
    
    // 현재 값이 맥스 값 보다 작을 때에만 while 진행
    while step.curr < step.max {
        //match 를 사용하여 증가 값을 보여주며 분기 적용.
        match step.next() {
            Some(v) => {
                println!("the current value is {:?}", v);
            }
            None => {
                println!("the step can not go higher anymore, the current value is {:?}, and max value is {:?}",                    step.curr, step.max);
            }
        }
    }
}

```




<!-- ### Stack Data Strutrue in Rust  -->
<!---->
<!--  what is a Stack ? -->
<!---->
<!--  * It is a list where items are added at one end and removed from the same endd -->
<!--  * For instance, like a stack of plates -->
<!--  * This mostly used in rapidly changing objects, parsers, and graphic processing like JavaScript Canvas operations -->



### Mutability, Variables, Copying, and Cloning

we are going to learn about copying and cloning 

* heap, and stack memory that can copy or not 
* type that implements copy trait and clone trait

```rust

// 일반적으로 러스트에서는 struct를 복사하는 것은 불가능하다. 다른언어와는 다르기 때문에 이점을 많이 어려워한다.
// 예제를 살펴보자.

#[derive(Debug)]
struct Person {
    name : String,
    age : i32,
}

fn main() {
    let p = Person {
        name: "Tony".to_string(),
        age: 35,
    };

    let p2 = p;

    println!("{:?}", p2);

    // p can not print because it is not impl copy trait. and String value is stored in the heap
    // memory. only stack memory implements copy trait automatically.
    
    // println!("{:?}", p);

    // but if we implements Clone trait, we can copy String into another section of memory

}

```


```rust

// 하지만 derive 라는 컴포넌트 자바에 @하고 Service 등 하는 것과 비슷하다. 
// derive 로 Clone 을 implements 하게 되면 해당 struct 과 String 필드는 Clone 이 가능하다. 
// 즉 struct 이 복사가 가능해진다.

#[derive(Debug, Clone)] //  String 타입은 Clone trait 을 impl 함 
struct Person {
    name : String, 
    age : i32,
}



fn main() {
    let mut p = Person {
        name: "Tony".to_string(),
        age: 35,
    };

    let p2 = p.clone();
    p.name.push_str("lee");
    println!("{:?}", p2);
    
    // p can not print because it is not impl copy trait. and String value is stored in the heap
    // memory. only stack memory implements copy trait automatically.

    println!("{:?}", p);

    // Copy 를 implements 한 타입 적용.
    #[derive(Debug, Clone, Copy)] // 안에 필드는 copy trait 를 impl 한 타입이라 copy 를 적용해야
                                 // 함. 단 Clone 도 해야하는데, 이유는 객체도 복사해야되기 때문. 
    struct Point {
        x : i32,
        y : i32,
    }

    let pnt = Point {x: 3, y: 3};

    let pnt2 = pnt; // .copy() 를 호출할 필요 없음 
   		    // copy trait 와 clone trait 의 차이점이 존재. -- copy 는 호출할 필요 없음.  
    println!("{:?}", pnt);
    
}
```
