# Generics, 제네릭

개인적으로 러스트에서 중요하게 생각하는 개념이 있는데 오너쉽, 트레이트, 그리고 제네릭 3가지라 생각한다.
그 중 제네릭에 대해 알아보자. 예제만 잘 숙달하여도 이해하는데 큰 문제는 없다고 생각한다. 

말 그대로 제네릭은 포괄적인 타입을 말한다. 그래서 제네릭 타입을 나타낼 때에도 타입을 특정하지 않고 `camel case` 방식으로 <Aaa, Bbb> 로 표현한다. 

### Functions & Implementation


* struct 에 제네릭 타입을 사용할 때에는 이름 뒤에 <T> 를 사용한다. 
* impl 에 사용할 떄에는 impl, Struct 뒤에 사용한다. 

하나의 struct 으로 여러 타입의 struct 인스턴스 생성이 가능하다. 

```rust,editable


#[derive(Debug)]
struct SGen<T> {
    random : T,
}

impl<T> SGen<T> {
    fn get_value(&self) -> &T {
            &self.random
    }
}


fn main() {  
       let gen = SGen { random : 3}; 
        println!("{:?}", &gen);
       let gen2 = SGen { random : 3.3};
        println!("{:?}", &gen2);
       let gen3 = SGen { random : "hello".to_string()};
        println!("{:?}", &gen3);
       let gen4 = SGen { random : "Hi"};
        println!("{:?}", &gen4);
}

```


### Traits 

trait 도 제네릭으로 활용할 수 있다. `Drop` `trait` 를 통해 알아보자.

```rust,editable


#[derive(Debug)]
struct Empty;

#[derive(Debug)]
struct Null;

trait DoubleDrop<T>  {
    fn double_drop(self, _: T);
}

// impl 할 struct 을 제네릭 타입 U 로 추가하였다. 현재 crate 에선 모든 타입이
// double_drop 을 사용할 수 있게 된다. 

impl<T, U> DoubleDrop<T> for U {
    fn double_drop(self, _: T) {}
}

fn main() { 
    let e = Empty;
    println!("{:?}", e);
    
    let n = Null; 
    println!("{:?}", n);

    // double_drop 함수 실행 후 더 이상 n, e는 사용할 수 없게 된다.
    n.double_drop(e);
    // println!("{:?}", n);
}


```


### Bounds 

제네릭의 타입을 정하고 제한하는 것을 바운드라고 한다.

바운드 표현 방식에 대해 알아보자

* 타입 뒤에 `:` 후 바운드
*  리턴 값 지정하기 전에 where 구문을 뒤에 바운드


```rust,editable


use std::fmt::Display;

// 타입 뒤에 바로 바운드
fn printer<T: Display>(t: &T) {
    println!("{}", t);
}

// 타입 지정 후 리턴 값 넣기 전에 where T : 방법으로 바운드
fn printer_<T>(t: &T)
where
    T: Display,
{
    println!("{}", t);
}


fn main() {
   
    //struct 자체에는 display 가 되지 않기에 따로 구현 필요. 
    struct NoDisplayTrait(i32);

    //생성 시점 부터 display 를 구현한 타입만 받을 수 있게 바운드 처리.
    struct NeedDisplayTrait<T: Display>(T);

    let string_p = "tony".to_string();


    printer(&string_p);


    printer_(&string_p);


    let nodisplay =NoDisplayTrait(3);
    
    // struct 은 display trait 이 구현되지 않았기에 printer 함수 실행 불가.
    
    // printer(&nodisplay);
   
    // vec 타입은 display trait 을 구현하지 않았기에 아예 생성 조차 불가능.

    // let vec_is_not_impl_display = NeedDisplayTrait(vec![1,2,3]);
   


}

```



예제를 통해 자세히 알아보자.

```rust,editable

struct Rectangle {
    length: f64,
    height: f64,
}

struct Triangle {
    length: f64,
    height: f64,
}

//trait HasArea 생성 후 함수만 생성
trait HasArea {

    fn area(&self) -> f64;

}


// HasArea 를 impl 한 Rec 
impl HasArea for Rectangle {

	// 함수식 구현
    fn area(&self) -> f64 {

        println!("this is rectangle");
        self.length * self.height
    
    }
}

// HasArea 를 impl 한 Tri 
impl HasArea for Triangle {

	// 함수식 구현
    fn area(&self) -> f64 {

        println!("this is triangle");
        (self.length * self.height) / 2.0
    
    }
}


// where 를 이용한 바운드
// 하나의 함수를 만들고 제네릭 타입을 받고 바운드 처리 후 
// 해당 trait 를 구현한 타입에 한해서 함수 실행.

fn get_area<T>(objec: &T) -> f64
where
    T: HasArea,
{
    objec.area()
}

fn main() {
    let rec = Rectangle {
        length: 3.0,
        height: 3.0,
    };

    let tri = Triangle {
        length: 3.0,
        height: 3.0,
    };

    let rec_area = get_area(&rec);
    println!("{:?}", rec_area);

    let tri_area = get_area(&tri);
    println!("{:?}", tri_area);
}

```


#### Testcase: empty bounds 

trait 에 함수가 없어도 유용하게 쓰일 수 있다. 예로 Copy 와 Eq trait 이 그러하다.

예제를 통해 어떻게 타입을 바운드 하는지 알아보자.

```rust,editable

#[derive(Debug)]
struct Rectangle {
    length: f64,
    height: f64,
}

struct Korea;
struct China;

trait Hanbok {}

// 이렇게 Korea 란 struct 을 impl 하는 것으로 해당 struct 의 타입은 Hanbok 으로 바운드 된다.
impl Hanbok for Korea {}


// 해당 함수는 Hanbok 이란 타입으로 제한하고 있다. 
fn ownership<T>(_: &T) -> &'static str
where
    T: Hanbok,
{
    "한복"
}

fn main() {
    let k = Korea;
    let c = China;


	// 함수 호출 불가능
    // println!("{:?}", ownership(&c));

	// 호출 가능    
    println!("{:?}", ownership(&k));
}

```







