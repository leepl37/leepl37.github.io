# Capturing

클로져는 타입을 지정하고 선언하는 데 있어 상당히 자유롭게 작용하며 대부분 컴파일러가 알아서 파악한다. 
즉 캡쳐링 함에 있어 해당 변수를 borrow 할지 mut borrow 할 지, move 할 지 클로져 함수에 따라 컴파일러가 알아서 알아낸다.


*  borrow &T 참조만 하는 경우.
*  mut borrow &mut T 변환가능하게 참조하는 경우.
*  T 데이터를 아예 가져오는 경우. 

크게는 이 세가지로 분류하며 이 경우를 잘 파악하고 인지하고 있어야 러스트를 깊이 있게 이해할 수 있다. 

예제를 통해 알아보자.


```rust,editable

fn main() {
    let color = String::from("red");
    // 캡쳐해서 변수의 값을 가져오는데, 이때는 & 참조로만 사용한다.
    let print = || println!("참조만 하기 -> {color}");
    // print() 클로져에서 color 를 참조로만 사용했기 때문에 다른 변수에 참조로 값을 선언할 수 있다.
    let t = &color;

    print();
    // 하지만 color 를 그대로 참조없이 다른 변수에 넣어버리면 더 이상 print() 클로져를 사용할 수
    // 없게 된다.  즉 아래 변수 선언은 컴파일 에러가 난다.
    // let move_color = color;
    print();

    let mut count = 0;
    // inc() 클로져의 경우 count 를 캡쳐 한 후 숫자 1 씩 추가하는 함수인데, 이렇게 하기 위해선
    // 단순히 borrow 가 아닌 &mut 또는 아예 가져와야하는데, 이럴 때에는 &mut 를 컴파일러가 알아서
    // 사용한다.
    let mut inc = || {
        count += 1;
        println!("&mut 변환가능한 참조 하기 -> increase count by 1, current number is : {count}");
    };

    inc();
    // inc() 클로져에서 이미 count 변수를 &mut 하게 사용하기 때문에 다른 변수에 선언할 수 없다.
    //let _count_borrowed = &mut count;
    inc();
    inc();

    //카피가 되지 않는 타입으로 선언. 힙에 저장된 변수.
    let movable = Box::new(3);

    //drop 함수는 std::lib 에 존재하는 함수인데, 데이터 객체 그 자체를 필요로 하기 때문에 아예
    //move한다.
    let consume = || {
        println!("객체를 아예 옮겨버리기 ->  movable : {:?}", movable);
        drop(movable);
    };

    consume();
    // 그렇기 때문에 아래 변수는 사용될 수 없다. 이미 consume 클로져에서 선언되었고 move했기
    // 때문이다. 모순 발생.
    //println!("{:?}", movable);

    let haystack = vec![1, 2, 3];
    
    //move 키워드를 사용하면 캡쳐링한 변수는 클로져로 이동하게 되어 사용할 수 없다.
    let contains = move |needle| haystack.contains(needle);

    println!("{}", contains(&3));
    //아래 haystack 을 프린트 하는 함수를 사용하면 컴파일 에러가 난다. 
    //println!("{:?}", haystack);
    println!("{}", contains(&4));
}

```

