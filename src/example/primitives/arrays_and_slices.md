# Arrays and Slices



### Arrays
* `Array` 는 스택 메모리에 인접하게 저장되어 있는 객체들을 말한다. 
* [] 을 사용하여 선언한다.
* [] 의 길이는 컴파일 타임에 알 수 있다(미리 선언되기 때문이며 Array 는 길이 변화가 불가능하기 때문)


```rust

fn main() {
	//배열 선언
	let arrays : [i32; 5] = [1,2,3,4,5];
	let ys : [i32; 500] = [0; 500];

	//배열 안에 데이터 접근
	println!("{}", arrays[0]);

}
```


### Slices
* `Slices` 는 `arrays` 와 비슷하지만 길이를 컴파일 타임에 알 수 없다. 
* `Slice` 는 두 단어로된 객체인데, 첫번째 단어는 저장된 데이터를 가르키는 포인터, 두번째 단어는 해당 포인터의 길이(usize)로 표현된다.


러스트의 참조 개념과 스트링 슬라이스에 대한 이해는 사실 초보자들에겐 쉽지만은 않다.
스스로도 무지한 상태에서 이를 이해하는데 꽤나 오랜 시간이 걸린 것 같다. 
눈으로 익혀 두고 기회가 되면 러스트 책 또는 다른 곳에서 이해할 수 있길 바란다.


```rust

let s = "hello world".to_string(); 

let world = &hello_world[6..11]; 

```
<img src="string_slice.svg" alt="drawing" width="300">




