# Cargo

`cargo` 공식적으로 러스트에서 지원하는 패키지 매니지먼트 툴이다. 

* Dependency 관리를 쉽게 할 수 있음. [crates.io](https://crates.io) - official package registry of rust
* 유닛테스트 지원
* 버전관리 용이


카고에 대한 자세한 내용은 [The Cargo Book](https://doc.rust-lang.org/cargo/) 참고하시길 바랍니다.



### Dependencies 


* binary project 생성 방법
* library project 생성 방법

```shell

# A binary
$cargo new foo

# OR A library
$cargo new --lib foo 

```



### Cargo Structure 

* Cargo.toml - config file 
* src - rs files

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
```


#### Cargo.toml

* name - 프로젝트 이름
* version - 버전 정보
* authors - 작성자
* dependencies - 추가할 디펜던시를 추가할 섹션 

디펜던스 사이트
[crates.io](https://crates.io)

```txt
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
clap = "2.27.1" # from crates.io 
rand = { git = "https://github.com/rust-lang-nursery/rand" } # from online repo
bar = { path = "../bar" } # from a path in the local filesystem


```

참고 사이트 about Cargo
[The Cargo book_format](https://doc.rust-lang.org/cargo/reference/manifest.html)


#### build & run 

* cargo build - 디펜덴시 다운로드 및 추가, 빌드
* cargo run - 빌드 후 실행



### Conventions, 바이너리 파일 실행

* main.rs 파일 외 다른 파일 실행하기 

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
    └── other
          └── other_main.rs
```


```shell

$cargo r --bin other_main

```
카고에 대한 내용이 생각보다 방대하고 알아두면 좋은 지식들이 많기에 사이트를 둘러보는 것을 추천한다.


### cargo test & Build scripts

추후 자세히 알아보자.






















