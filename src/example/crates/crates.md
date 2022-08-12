# Crates


* crate 은 컴파일 유닛 - rustc some.rs 가 실행되면 해당 파일은 crate file 로 여겨진다.

* crate 은 바이너리 또는 라이브러리 파일로 컴파일되며 default 는 바이너리. `--crate-type` flag 를 통해 지정도 가능. 



### creating a library file.

rary.rs 라는 파일 만든 후에 아래 코드를 작성한다. 

```rust

pub fn rary_fn() {
	println!(" rary function ");
}

```


shell 에서 라이브러리 타입으로 컴파일한다. 
lib은 default, prefix name 이고 뒤에 붙는 것이 우리가 생성한 파일 이름이다. 
* `--crate-name` 옵션으로 변경 가능.


```shell
$ rustc --crate-type=lib rary.rs 
$ ls lib*

library.rlib

```


### Using a Library 

rlib 파일을 --extern 이름 지정 = library.rlib 에 추가하여 실행할 파일 추가한다. 
해당 파일에서 라이브러리 사용 가능.


```rust

fn main() {

	rary::public_fn();

}

```
```shell

$ rustc main.rs --extern rary=library.rlib --edition=2021 && ./main

```



