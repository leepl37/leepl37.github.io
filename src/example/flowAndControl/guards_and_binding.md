# Guards/Binding


### Guards
매치에서 쓰이는 guard 는 filter 의 기능으로 사용할 수 있다.

```rust,editable

fn main() {
	
	let pair = (3, 3);

	println!("the pair is {:?}", pair);

	match pair {
		(x, y) if x > y => println!("x is bigger than y "),
		(x, y) if x == y => println!("x is same as y"),
		_ => println!("x is smaller than y"),
	}

}

```

### Binding
말 그대로 변수에 특정 value 를 @ 을 통해 바인딩 할 수 있다.

```rust,editable

fn get_value() -> u32 {
	33
}
fn main() {
	// return 값이 u32 이기 때문에 u32 에 대한 모든 수를 매치 시켜줘야 한다. 말이 된다?
	match get_value() {
		n @ 1 ..=19 => println!("미성년"),
		n @ 20 ..=30 => println!("성인, 이립"),
		n @ 31 ..=40 => println!("어른, 불혹"),
		n @ 41 ..=50 => println!("지천명"),
		n @ 51 ..=60 => println!("이순"),
		n @ 61 ..=70 => println!("종심"),
		_ => println!("망구"),

	}
}

```


