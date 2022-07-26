# Formatted print, 형식에 맞춰 출력하기.

러스트에서 print 는 macros 로 짜여져있다. 후반에 macro 에 대해 자세히 다룰 예정이다. 
또한 macro 는 standard lib 인 std::fmt 에 포함되어 있다.

* 포맷 종류
	* `format!`: 어떠한 text 를 String 으로 포맷팅한다. 
	* `print!`: 포맷과 비슷하지만 입력된 text 는 콘솔에 표현된다 (io::stdout).
	* `println!`: print 와 동일하지만 ln 을 보면 알겠지만 한 줄 추가된다(엔터 친 효과).
	* `eprint!`: 포맷과 같지만 text가 standard error 로 프린트된다(io::stderr).
	* `eprintln!`: eprint!와 같지만 한 줄 추가된다.


```rust 
fn main() {

        let a = "ln이 없을 경우와 있을 경우.".to_string(); 
        let b = "ln이 있을 경우와 없을 경우.".to_string();
        print!("{a} :");
        print!("한 줄 추가 없이 그대로 나열됨.\n"); 
        println!("-----------------------------");
        println!("{b} :");
        print!("한 줄 추가된 채로 나열됨. \n ");
     

        println!("{1} 보다 {0} 을 먼저 갖춰라.", "인", "예");
 
        //변수 이름 지정
        println!("{first}은 바람을 거역해서  {second} 를 낼 수 없지만, {third}이 풍기는 향기는 바람을 거역해서 사방으로 퍼진다.",   
              first="꽃",    
              second="향기",  
              third="선하고 어진 사람");    
   
   	
		// : 를 이용해서 포맷하기.
        println!("10 진수 {}",   69420);
        println!(" 2 진수 {:b}", 69420);
        println!(" 8 진수 {:o}", 69420);
        println!("16 진수 {:x}", 69420);
   
   
        // 공백 나타내기.
        println!("{number:>5}", number=1);
   
        // 공백 값 지정하기.
        println!("{number:0>5}", number=1);
   
        //공백 값을 변수에 지정하기.
        println!("{number:0>width$}", number=1, width=5);

}


