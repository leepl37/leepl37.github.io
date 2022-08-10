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
