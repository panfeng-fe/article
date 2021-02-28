# rust学习

## 1、数据类型

```rust
fn main() {
    // bool 布尔型
    let is_ture: bool = true;
    let is_false: bool = false;
    println!("is_true = {} ,is_false = {}",is_ture,is_false);

    // char 字符串类型(默认32位，可以是汉字)
    let chara: char = 'a';
    let charb: char = '潘';
    println!("chara = {}, charb = {}",chara,charb);

    // number 数a字类型
    // 种类：i8,i16,i32,i64,u8,u16,u32,u64
    let numbera: i8 = -1;
    let numberb: u8 = 1;
    println!("numbera = {}, numberb = {}",numbera,numberb);

    // 自适应类型 isize,usize
    println!("max = {}", usize::max_value());

    // 数组 size时数组类型的一部分，数组数量需要保持一致
    // [type:size]
    let arr:[u32;5] = [1,2,3,4,5];
    println!("arr[0] = {}",arr[0]);
    show(arr);

    // 元组
    let tup:(i32,char) = (1,'潘');
    println!("item1 is {}",tup.0);
    println!("item2 is {}",tup.1);

    // 元组解构
    let (tupa,tupb) = tup;
    println!("tupa = {},tupb = {}",tupa,tupb);

    // 函数
    let sub_number:u32 = sub(1,2);
    println!("sub_number = {}",sub_number);


    // 逻辑语句
    let if_else = 0;
    if if_else == 1 {
        println!("if_else = 1");
    } else {
        println!("if_else != 1");
    };

    // 循环loop
    let mut counter = 0;
    loop {
        if counter > 10 {
            break;
        }
        counter += 1;
        println!("now counter is {}",counter);
    }
    
        let mut counter = 0;

    // loop 循环带返回值
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter*2;
        };
    };
    println!("result is {}",result);

    // while循环
    while counter != 10 {
        counter += 1;
    };
    println!("i = {}",counter);
    
    // for 循环
    let arr:[u32;5] = [1,2,3,4,5];
    for item in arr.iter() {
        println!("items is {}",item);
    }

    // or 
    for item in &arr {
        println!("items is {}",item);
    }
}

fn show(arr:[u32;5]){
    for i in &arr {
        println!("item is {}",i);
    }
}

fn sub(a:u32, b:u32) -> u32{
    a + b
}

```

