# Examples in std, standard Lib 에 존재하는 예제

### Iterator::any, iter에 있는 any라는 함수에 대해

any라는 함수는 해당 iter 객체에서 나온 element 가 예상한 어떤 value 값과 비교하여 맞으면 true 아니면 false 를 반환하는 함수이다.

std::lib 에서는 어떻게 구현했는지 알아보자.

```rust,editable 

//std::lib 에 구현된 iter trait 
pub trait Iterator {
    //해당 iter의 type 을 item 이라 지정한다.
    type Item;
    
    //any 함수는 자신을 파라미터로 받고 클로져도 파라미터로 받는다. return 값은 bool type. 
    fn any<F>(&mut self, f: F) -> bool
    where
        // 클로져에 대한 타입과 return 값으 지정한다. 클로저 파라미터 또한 해당 iter 객체의
        // 아이템인 것을 알 수 있다. 왜 이렇게 하는가?
        F: FnMut(Self::Item) -> bool;
}

fn main() {
    let any_test = vec![1, 2, 3, 4, 5];
    // iter 로 변환할 수 있는데 Vec 타입의 변수를 구현하고
    // any 함수를 호출한다. 이때 any 함수는 파라미터로 자기 자신과 클로져를 받는다는 것을 알 수있다.
    // 호출할 때 자기 자신이 파라미터로 포함되었다. 그럼 클로져만 파라미터로 넣으면 된다.
    // || 를 사용하여 파라미터를 클로져를 구현하고 뒤에 리턴 값인 bool type 이 나올 수 있게 식을
    // 구현한다.
    let answer = any_test.iter().any(|&x| x == 2);

    println!("{answer}");
}


```


### Iterator::find, iter에 있는 find 라는 함수에 대해
```rust,editable

trait Iterator {
    //해당 iter의 type 을 item 이라 지정한다.
    type Item;

    fn find<P>(&mut self, p: P) -> Option<Self::Item>
    where
        //find function 의 클로져는 iter 의 아이템을 & 로 받음.
        P: FnMut(&Self::Item) -> bool;

    fn any<F>(&mut self, f: F) -> bool
    where
        //any function 의 클로져는 iter 의 아이템을 & 없이 받음.
        F: FnMut(Self::Item) -> bool;
}

fn main() {
    let string_list = vec!["hi".to_string(), "bye".to_string()];
    //iter 를 통해 |x| 는 &string 이 된다
    let answer_any = string_list.iter().any(|x| x == "hi" );
    println!("{}", answer_any);
    let str_list = vec!["hi", "bye"];
    // iter 를 통해 |x| &&str 가 되고  == str에 &를 붙이면 && 가 되므로 비교 가능
    let str = str_list.iter().any(|x| x == &"hi");
    println!("{}", str);
    let num_list = vec![1,2,3,4,5];
    // iter 를 통해 |x| &i32 가 되고 == i32 에 &를 붙이면 &i32 == &i32 되므로 비교 가능
    let num = num_list.iter().any(|x| x == &3);
    println!("{}", num);
    // find fn 는 &를 추가한 item 값을 받는데 이유는 return 해야하기 때문이다.
    // &&string 이 되므로 == &&str 이랑 비교한다.
    let answer_find = string_list.iter().find(|&x| x == &"hi");
    println!("{:?}", answer_find);
    let str_list = vec!["hi", "bye"];
    //iter() 를 통해 &str 가 되고 find 는 &를 추가한 item 을 받기에 &x 추가한다. 
    let find_str_list = str_list.iter().find(|&x| x == &"hi");
    println!("find_str_list : {:?}", find_str_list);
    let fin_num_list = num_list.iter().find(|&x| x == &3);

    // iter 을 통해 |x| 값은 각 각 &String, &&str, &i32 가 된다. 
    // &str 와 i32 는 Copy type 이다. &x pattern 에 의해 destruct 된다.
}

```





