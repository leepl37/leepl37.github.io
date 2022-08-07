# if let / while let


match 보다 간결하게 쓸 수 있다.

```rust,editable

fn main() {
	let optional = Some(7);

	match optional {
		Some(i) => println!("use match, value is {}", i),
		_ => println!("none"),
	}

	let use_if_let = Some(7);

	if let Some(i) = use_if_let {
		println!("use if let, value is : {}", i);
	} else {
		println!("none");
	}
}

```


```rust,editable

fn main() {
    
    let mut value = Some(0);
    // 굳이 설명하자면 while let 은 Some(i) 인 형태일 때까지 구문을 실행한다.
    while let Some(i) = value {
        if i  > 9 {
            println!("got 10!!, quit");
            value = None
        }else{
            println!(" not reached yet... value is {}", i);
            value = Some(i+1)
        }

    }
}
```

