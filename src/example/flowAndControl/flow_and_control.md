# Flow and Control

* if - else 
```rust, editable

fn main() {
    let t = 3;

    let init: i32;

    if t > 3 {
        init = 4;
    } else if t < 3 {
        init = 2
    } else {
        init = 30
    }
    println!("init value : {init}");

    let return_value = if init > 40 {
        50
    } else if init == 30 {
        init * 10
    } else {
        init / 10
    };

    println!("return value : {return_value}")
}


```

* loop 

```rust, editable

fn main() {

	let mut t = 3i32;

	loop {
		t += 1;
		if t == 10 {
			println!("t is finally 10");
			continue;
		}
		
		if t < 10 {
			println!("t is not yet 10, t is {}", t);
		}

		if t == 11 {
			println!("bye..");
			break;
		} 
	}
}
```


* while 

```rust

fn main() {

	let mut n = 1u32;

	while n < 50 {
		if n % 2 == 0 {
		 	println!("짝 : {n}");
		}else{
			println!("홀 : {n}");
		}
		n += 1;	
	}

}

```


