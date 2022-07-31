# TestCase: linked-list

흔히들 사용하는 간단한 링크드 리스트 배열을 구현해보자. 
내용이 어렵기 때문에 설명은 간략하게 할 것이며 눈으로 익히는 정도만 해도 충분하다. 

```rust

#[derive(Debug)]
enum List {
    //Box 를 사용하는 것은 메모리를 정확히 알 수 없을 경우 사용한다. 힙에 저장됨.
    // List 가 몇개나 들어갈지 알 수 없기 때문에 Box 사용하며 자세한 내용은 뒤에서 설명하겠다.
    Cons(u32, Box<List>),
    Nil,
}

impl List {
	
    // 배열 첫번째에 숫자를 넣는 경우, 객체 그 자체가 새로 만들어지기 때문에 & 없이 self 변수가 들어간다.
    fn insert_to_fth(self, fth_num: u32) -> Self {
        List::Cons(fth_num, Box::new(self))
    }
	
    // associated function 이라고 하는데 객체의 구현 시, 파라미터가 없이 사용할 수 있는 함수이다. static method 와 비슷한 개념이다.
    fn new() -> Self {
        List::Nil
    }
    
    // 해당 리스트의 길이를 알려주는 함수인데 tail.len() 재귀함수 방식으로 구현한다. 
    // match 와 래퍼런스 개념이 필요한데 눈으로 익히고 추후에 자세하게 설명하겠다.
    // 간단하게 설명하면 객체 자체를 파라미터로 넣으면 해당 객체는 더이상 사용할 수 없게 됨으로 참조하여 사용하며
    // match 사용 시, 러스트 컴파일러는 참조된 것으로 파악해서 match &self 로 쓰지 않아도 &self 된 것 처럼 사용할 수 있다.
    fn len(&self) -> u32 {
        match self {
            List::Cons(_, tail) => 1 + tail.len(),
            List::Nil => 0,
        }
    }

    
    fn stringify(&self) -> String {
        match self {
            List::Cons(head, tail) => {
                    format!("{}  {} ", head, tail.stringify())
            },
            List::Nil => {
                format!("")
            }
        }
    }
}

fn main() {
    let t_list = List::new();
    println!("{:?}", t_list);

    let mut new_list = t_list.insert_to_fth(3);
    println!("{:?}", new_list);
    for n in 1..10 {
        new_list = new_list.insert_to_fth(n);
        match &new_list {
            List::Cons(_, ref tail) => {
                println!("List : {:?} : len : {:?}", new_list, tail.len());
            }
            List::Nil => todo!(),
        }
    }
    let print = new_list.stringify();
    println!("{}", print);
}

```


