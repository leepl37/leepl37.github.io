# Comments, 주석

* 주석은 컴파일러에 의해 무시된다. 즉 사용자가 코드에 대한 설명 등을 작성할 때 주로 사용된다. 

* 종류
	* // 라인 주석
	* /* 블록 주석


```rust
fn main() {

	/*
	 * 설명이 긴 주석이 필요로 할 때에는 블록 주석을 
	 * 통해 추가할 수 있습니다.
	 */


	 // 변수 a 와 b 를 통해 원하는 String 을 나타낼 수 있다.
         let a = String::from("러스트를 배워보자"); 
         let b = String::from("예제를 통해"); 
         println!("{} {} ", b, a)	

}

```

