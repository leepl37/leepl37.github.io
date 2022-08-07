# Methods

### Associated functions & Methods 

러스트에서 Associated fn 과 Methods 라고 불리는 함수가 존재한다. 
자바의 static method 라고 불리는 것이 Associated fn 이다.

객체 생성없이 해당 객체의 함수를 호출할 수 있는 함수를 Associated fn 이라 하며 생성된 객체, 인스턴스의 함수를
method 라 한다. 

예를 통해 살펴보자.



```rust,editable

#[derive(Debug)]
struct Point {
    x : f64,
    y : f64
}

impl Point {
	//Associated fn 객체 생성 없이 호출 가능.
    fn new() -> Point {
        Point { x : 0.0, y : 0.0}
    }

	// method 생성된 객체, 인스턴스에서 호출할 수 있는 함수.
    fn transrate(&mut self, x: f64, y: f64) {
        self.x = x;
        self.y = y;
    }
	// move 개념인데 눈으로만 익히고 넘어가면 곧 자세히 설명할 수 있는 챕터가 나온다. 
    fn drop_point(self) {
        let move_to_here = self;
        println!("point was dropped");
    }
}



fn main() {
    
     let mut point = Point::new();
     
     println!("new : {:?}", point);
     
     point.transrate(3.3, 3.3);
     
     println!("transrate : {:?}", point);

     point.drop_point();
	// 이미 move 된 후 drop 되었기 때문에 해당 인스턴스는 메모리에서 없어짐.
     // println!("can't call this {:?}", point);

}
```
