# for and iterators

for문과 iterators에 대해 알아보자.

### For 문 

```rust

fn main() {

	for n in 1..101 {
		println!("{n}");
	}

	for n in 1..=100 {
		
		println!(" 숫자 100까지 출력 {n}");

	} 

}
```


### Iterators 

iterators 형은 for문 형태로 적용이 가능한 포맷을 말한다. 다시 말하면 loop 문이 가능하다. 
또한 러스트에는 iter 종류가 3가지 존재한다. 


* `iter` - borrows, 데이터를 참조만 할 수 있다.
* `iter_mut` - mutably borrows, 변환 가능하게 참조한다. 
* `into_iter` - consumes, 러스트에서 데이터에서 끄내온 객체는 더 이상 그 객체의 데이터는 사용이 불가능하다. 
해당 소비된 데이터는 move 이동하였기 때문이다.



#### iter()
```rust
fn main() {
    let name1 = vec!["hell", "low", "world"];

    for n in name1.iter() {
        // 참조로 사용하는 중
        match n {
            &"hell" => {
                println!("name : {}", n);
            }
            _ => {
                println!("others");
            }
        }
    }
}
```


#### iter_mut()
```rust
fn main(){

    let mut name2 = vec!["hell", "low", "world"];
    for (index,n) in name2.iter_mut().enumerate() {
        // mutable 하게 사용 가능.
	// *는 ref된 데이터를 온전한 데이터로 사용한다는 것을 의미한다. 즉 온전한 데이터이기 때문에 다른 value 값을 넣을 수 있다.
        *n = match n {
            &mut "world" => "tony",
            _ => if index == 0 {
                "finally"
            }else {
                "i found "
            }
        }

    }
    println!("{:?}", name2);

}
```

#### into_iter()
```rust

fn main() {
    
    let name3 = vec!["hell", "low", "world"];

    for n in name3.into_iter() {
    	//데이터 이동 중...
        match n {
           "world" => println!("hii"),
            _ => println!("...")
        }
    }
    //move, consume 되었기 때문에 더 이상 사용이 불가능하다.
    //println!("{:?}", name3);
}

```



















