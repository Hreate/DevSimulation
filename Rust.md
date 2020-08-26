# Rust

## 1. 准备工作

### 创建工程

```shell
cargo new 项目名
```

### 运行

```shell
cargo run
```

### 编译

```shell
cargo build
```

### 语法检查

```shell
cargo check
```



## 2. Rust 基础

### 2.1 通用编程概念

#### 2.1.1 变量（可类型推导）：let name: type;

* ##### 可变性

  定义变量用 let ，如果变量没有用 mut 修饰，默认是不可变的，let mut name: type;

* ##### 常量

  const name: type;

* ##### 隐藏

  定义同名变量时，新定义的变量会在之后的作用域内将旧的隐藏，旧的变量在其作用域内依旧有效



#### 2.1.2 数据类型

1. ##### 基础数据类型

   * bool

   * char

     32位，let a = '你';

   * 数字类型

     i8，i16，i32，i64

     u8，u16，u32，u64

     f32，f64

   * 数组

     [Type; size]，size 也是数组类型的一部分

     let arr:[u32; 5] = [1, 2, 3, 4, 5];

   * 自适应类型

     isize

     usize

2. ##### 复合数据类型

   * 元组

     ```rust
     let tup: (i32, f32, char) = (-3, 3.69, '好');
     let tup = (-3, 3.69, '好');
     println!("{}", tup.0)
     let (x, y, z) = tup; // 元组拆解，x、y、z三个变量分别被赋值
     ```

   * 结构体

   * 枚举

3. ##### 字符串类型



#### 2.1.3 函数

##### 三种写法：

```rust
fn fun(a: i32, b: i32) -> i32 {
    return a + b;
}

fn fun(a: i32, b: i32) -> i32 {
    return a + b
}

fn fun(a: i32, b: i32) -> i32 {
    a + b
}
```

##### 注意：

![](D:\BNA\note\imgs\批注 2020-08-18 205742.png)



#### 2.1.4 注释

* //



#### 2.1.5 控制流

* ##### if，似 Go

* ##### if - else，似 Go

* ##### if - else if - else，似Go

  * 在 let 中使用 if

    ```rust
    let condition = true;
    let x == if condition {
        5
    } else {
        6
        // "six", error
    }
    ```

    

* ##### loop

  ```rust
  let mut counter = 0;
  loop {
      println!("in loop");
      if counter == 10 {
          break;
      }
      // counter = counter + 1;
      counter += 1;
  }
  let result = loop {
      counter += 1;
      if counter == 20 {
          break counter * 2;
      }
  };
  println!("result = {}", result);
  ```

* ##### while

  ```rust
  let mut i = 0;
  while i != 10 {
      i += 1;
  }
  println!("i = {}", i);
  ```

* ##### for

  ```rust
  let arr:[u32; 5] = [1, 2, 3, 4, 5];
  for element in arr.iter() {
      println!("element = {}", element);
  }
  for element in &arr {
      println!("element = {}", element);
  }
  ```



### 2.2 所有权

rust 通过所有权机制来管理内存，编译器在编译时就会根据所有权规则对内存的使用进行检查。



#### 2.2.1 堆和栈

编译时，数据类型的大小是固定的，就是分配在栈上；

编译时，数据类型的大小不固定，就是分配在堆上。



#### 2.2.2 作用域：{}



#### 2.2.3 String内存回收

String类型离开作用域时，会调用drop方法。



#### 2.2.4 移动

##### 浅拷贝，并且所有权转移，原变量不再指向对应堆内存地址：

```rust
let a= String::from("shit");
let b = a;
println!("b = {}", b);
println!("a = {}", a); // error, move to b, a invalid
```



#### 2.2.5 clone

##### 深拷贝：

```rust
let a= String::from("shit");
let b = a.clone();
println!("b = {}", b);
println!("a = {}", a);
```



#### 2.2.6 栈上数据拷贝（copy trait）

##### 实现了 copy 特征，就是深拷贝：

```rust
let a = 1;
let b = a;
println!("a = {}, b = {}", a, b)
```



##### 常用的具有 copy trait 的类型：

* 所有的整形
* 浮点型
* 布尔值
* 字符类型
* 元组



#### 2.2.7 函数和作用域

引用类型作参数传入时，会发生移动，所有权转移给函数内的参数，函数结束后参数销毁，所有权销毁。



#### 2.2.8 引用（不可变引用）：&

##### 创建一个指向值的引用，但是并不拥有它，因为不拥有这个值，所以，当引用离开其值指向的作用域后也不会被丢弃：

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("s1 = {}, len = {}", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

##### 注意：

* 就是在函数执行时创建了一个指针的指针，函数结束后，把指针的指针丢弃了
* 引用不可修改值



#### 2.2.9 借用（可变引用）：&mut

一个变量只能借用一次，借用后，在之后的同一作用域内，原变量及其（不可变）引用不可使用。

##### 理解：

多个读，单个写。类似于读写锁。



#### 2.2.10 slice

##### 字符串 slice 是 String 中一部分值的引用

```rust
fn main() {
    let s = String::from("hello world");
    // 第一种写法，左闭右开
    // let h = &s[0..5];
    // 第二种写法，闭区间
    // let h = &s[0..=4];
    // 第三种写法，默认从头开始
    // let h = &s[..=4];
    // 第四种写法，默认到末尾
    let h = &s[0..];
    println!("h = {}", h);
}
```



##### 字面值就是 slice（&str）

```rust
fn main() {
    let mut h = "iii";
    h = "mmm";
    println!("h = {}", h);
}
```



##### 其它类型 slice

* 数组 slice

  ```rust
  fn main() {
      let a = [1, 2, 3, 4];
      let h = &a[..3];
      println!("h = {:?}", h); // 完整打印
  }
  ```




### 2.2 结构体

#### 2.2.1 定义结构体

```rust
#[derive(Debug)]
struct User {
    name: String,
    count: String,
    nonce: u64,
    active: bool,
}
```



#### 2.2.2 创建结构体实例

```rust
let xiaoming = User {
    name: String::from("xiaoming"),
    count: String::from("80001000"),
    nonce: 10000,
    active: true
};
```



#### 2.2.3 修改结构体字段

```rust
let mut xiaohuang = User {
    name: String::from("xiaohuang"),
    count: String::from("80001000"),
    nonce: 10000,
    active: true
};
xiaohuang.nonce = 20000;
```



#### 2.2.4 参数名字和字段名字同名的简写方法

```rust
let name = String::from("xiaoxiao");
let count = String::from("89077777");
let nonce = 200000;
let active = false;

let user1 = User {
    name,
    count,
    nonce,
    active
};
```



#### 2.2.5 从其他结构体创建实例

```rust
let user2 = User {
    name: String::from("user2"), // 重新给 name 字段赋值，其余字段赋予与 user1 相对应的值。因为 user2 的 name 字段是重新赋值，所以 user1 的 name 字段的所有权没有进行转移，依旧可以使用。
    ..user1 // 使用 user1 给 user2 赋值后，引用类型字段的所有权会转移，user1 中引用类型字段将不能再使用，非引用类型字段仍可以使用。
};
println!("{:?}", user2);
```



#### 2.2.6 元组结构体

* 字段没有名字
* 圆括号

```rust
#[derive(Debug)]
struct Point(i32, i32);
let a = Point(10, 20);
println!("a.x = {}, a.y = {}", a.0, a.1);
println!("{:?}", a);
```



#### 2.2.7 没有任何字段的类单元结构体

类比于面向对象语言，类中没有成员变量，但是可以有成员函数。

```rust
struct A;
struct B{};
```



#### 2.2.8 打印结构体

```rust
#[derive(Debug)] // 在结构体声明上
println!("{:?}", a); // 完整打印结构体实例
println!("{:#?}", a); // 完整打印结构体实例，字段自动换行
```



#### 2.2.9 方法

```rust
#[derive(Debug)]
struct Dog {
    name: String,
    weight: f32,
    height: f32
}

impl Dog {
    // self 类似于 java 中的 this
    fn get_name(&self) -> &str {
        &(self.name[..])
    }

    fn get_weight(&self) -> f32 {
        self.weight
    }

    // fn get_height(&self) -> f32 {
    //     self.height
    // }

    fn yap() {
        println!("oh oh oh");
    }
}

impl Dog {
    fn get_height(&self) -> f32 {
        self.height
    }
}

fn main() {
    let dog = Dog {
        name: String::from("diudiu"),
        weight: 12.0,
        height: 20.0
    };
    println!("dog = {:#?}", dog);
    println!("name = {}", dog.get_name());
    println!("weight = {}", dog.get_weight());
    println!("height = {}", dog.get_height());
    Dog::yap();
    println!("Hello, world!");
}
```



### 2.3 枚举与模式匹配

#### 2.3.1 类似于c语言的方式定义

```rust
enum IpAddrKind {
    V4,
    V6
}
struct IpAddr {
    kind: IpAddrKind,
    address: String
}
let i1 = IpAddr {
    kind: IpAddrKind::V4,
    address: String::from("127.0.0.1")
};
```



#### 2.3.2 rust语言提倡的方式定义

```rust
enum IpAddr2 {
    V4(String),
    V6(String)
}
let i1 = IpAddr2::V4(String::from("127.0.0.1"));
```



#### 2.3.3 可以是不同类型

```rust
enum IpAddr3 {
    V4(u8, u8, u8, u8), // 元组
    V6(String)
}
let i1 = IpAddr3::V4(127, 0, 0, 1);
```



#### 2.3.4 经典用法

```rust
enum Message {
    Quit,
    Move{x: i32, y: i32},
    Write(String),
    Change(i32, i32, i32)
}
// 每项分别等同于
// struct Quit; // 类单元结构体
// struct Move {
// x: i32,
// y: i32,
// }
// struct Write(String)
// struct Change(i32, i32, i32)
```



#### 2.3.5 枚举类型的方法以及match

```rust
impl Message {
    fn show(&self) {
        match self {
            Message::Quit => println!("Quit"),
            Message::Move {x, y} => println!("Move x = {}, Move y = {}", x, y),
            Message::Change(a, b, c) => println!("Change a = {}, Change a = {}, Change a = {}", a, b, c),
            // _ => println!("Write"), // 其他情况匹配
            Message::Write(s) => println!("Write = {}", s)
        }
    }
}

fn main() {
    let quit = Message::Quit;
    quit.show();
    
    let mo = Message::Move{x: 10, y: 20};
    mo.show();
    
    let wri = Message::Write(String::from("Hello"));
    wri.show();

    let cha = Message::Change(1, 2, 3);
    cha.show();
}
```

