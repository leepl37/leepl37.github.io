# A Freestanding Rust Binary

먼저 러스트 OS 를 구현하기 위해서는 executable 한 파일을 만들되 어떤 standard library와도 연결되지 않아야 한다. 

이러한 파일을 Freestanding 혹은 bare-metal 이라고 한다. 

#### Introduction 

커널을 만들기 위해서는 어떠한 시스템의 요소들에게도 의존하지 않아야 한다. (쓰레드, 파일, 힙 메모리, 네트워크, 출력 또는 하드웨어 등)

다행히 이터레이터, 클로져, 패턴 매칭, 옵션, Result, 스트링 포맷팅, 오너쉽 시스템은 사용할 수 있는데 이러한 특징들만 사용해도 메모리 안정성과 알 수 없이 일어나는 문제들에 대해 많이 해결할 수 있다. 


#### Disabling the Standard library

stdlib 의존 제거 하기.

```shell

cargo new blog_os --bin --edition 2018

```

executable 파일을 만들기 위해 cargo 를 통하여 프로젝트를 만든다. 

```rust
#![no_std]

fn main() {
    println!("can not print this");
}

```
