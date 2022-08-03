# casting_and_literals

### Casting 

```rust,editable
fn main() {
    let decimal = 65.4321_f32;

    //let integer : u8 = decimal; u8 이라는 타입을 지정하여도 value 값에 지정한 게 아니기 때문에
    //명확하게 표현된 것이라 보기 어렵다.

    let integer = decimal as u8; // value 값에 변환 타입을 명확히 지정
    let character = integer as char;

    // decimal 에서 바로 char 로 변환하는 것은 가능한가
    //let char = decimal as char;  //소수에서는 utf8 로 매칭되는 게 없기 때문에 불가능하다.

    println!("Casting : {decimal} -> {integer} -> {character}"); // 가능


    // 8비트에서 16이나 32비트로 형변환 하는 것은 문제가 없다. 그렇다면 16비트에서 8비트, 더 적은
    // 비트 수로 변환하는 것은 어떻게 될까
   
    let bit_16 = 300.0_f32;

    let answer = bit_16 as u8;
   
    // unsigned 8, u8은 255 까지 밖에 표현되지 못하기 때문에 300이 나오지 못한다.
    println!("{answer}");
  
    // overflow. unsafe 는 자주 사용되진 않지만 시스템 쪽을 구현해야하 할때 종종 사용하는 듯 하다.
    // runtime cost가 존재하기 때문에 완벽히 이해한 후 사용해야한다.
    unsafe {
        // 300.0 is 44
        println!("300.0 is {}", bit_16.to_int_unchecked::<u8>());
    }

}

```

```rust,editable

fn main() {

    // value 값과 타입을 한번에 지정한 literals
    let x = 1u8;
    let y = 2u32;
    let z = 3f32;
    
    // 타입을 지정하진 않고 사용됨에 따라 타입이 지정됨.
    let i = 1;
    let f = 1.0;

    println!("size of `x` in bytes: {}", std::mem::size_of_val(&x));
    println!("size of `y` in bytes: {}", std::mem::size_of_val(&y));
    println!("size of `z` in bytes: {}", std::mem::size_of_val(&z));
    println!("size of `i` in bytes: {}", std::mem::size_of_val(&i));
    println!("size of `f` in bytes: {}", std::mem::size_of_val(&f));
}
```

