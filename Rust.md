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
    active: true,
};
```



#### 2.2.3 修改结构体字段

```rust
let mut xiaohuang = User {
    name: String::from("xiaohuang"),
    count: String::from("80001000"),
    nonce: 10000,
    active: true,
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
    active,
};
```



#### 2.2.5 从其他结构体创建实例

```rust
let user2 = User {
    name: String::from("user2"), // 重新给 name 字段赋值，其余字段赋予与 user1 相对应的值。因为 user2 的 name 字段是重新赋值，所以 user1 的 name 字段的所有权没有进行转移，依旧可以使用。
    ..user1, // 使用 user1 给 user2 赋值后，引用类型字段的所有权会转移，user1 中引用类型字段将不能再使用，非引用类型字段仍可以使用。
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
    height: f32,
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
        height: 20.0,
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
    V6,
}
struct IpAddr {
    kind: IpAddrKind,
    address: String,
}
let i1 = IpAddr {
    kind: IpAddrKind::V4,
    address: String::from("127.0.0.1"),
};
```



#### 2.3.2 rust语言提倡的方式定义

```rust
enum IpAddr2 {
    V4(String),
    V6(String),
}
let i1 = IpAddr2::V4(String::from("127.0.0.1"));
```



#### 2.3.3 可以是不同类型

```rust
enum IpAddr3 {
    V4(u8, u8, u8, u8), // 元组
    V6(String),
}
let i1 = IpAddr3::V4(127, 0, 0, 1);
```



#### 2.3.4 经典用法

```rust
enum Message {
    Quit,
    Move{x: i32, y: i32},
    Write(String),
    Change(i32, i32, i32),
}
// 每项分别等同于
// struct Quit; // 类单元结构体
// struct Move {
// x: i32,
// y: i32,
// };
// struct Write(String);
// struct Change(i32, i32, i32);
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



#### 2.3.6 Option

1. ##### Option是标准库定义的一个枚举，形式：

   ```rust
   enum Option<T> {
       Some(T),
       None,
   }
   ```

2. ##### 使用方式

   ```rust
   fn main() {
       let some_number = Some(5);
       let some_string = Some(String::from("a string"));
       let absent_number : Option<i32> = None;
   
       let x: i32 = 5;
       let y: Option<i32> = Some(5);
       let mut temp = 0;
       match y {
           Some(i) => temp = i,
           None => println!("do nothing"),
       }
       let sum = x + temp;
       println!("sum = {}", sum);
   
       let result = plus_one(&y);
       match result {
           Some(i) => println!("result = {}", i),
           None => println!("nothing"),
       }
   
       if let Some(value) = plus_one(&absent_number) {
           println!("value = {}", value);
       } else {
           println!("do nothing");
       }
   
       println!("y = {}", y.unwrap());
   
       println!("Hello World!");
   }
   
   fn plus_one(x: &Option<i32>) -> Option<i32> {
       match x {
           None => None,
           Some(x) => Some(x+1),
       }
   }
   ```



### 2.4 Vector

#### 2.4.1 创建空的vector

```rust
let mut v: Vec<i32> = Vec::new();
```



#### 2.4.2 创建包含初始值的vector

```rust
// 使用宏创建包含初始值的vector
let v = vec![1, 2, 3];
println!("v = {:?}", v);
```



#### 2.4.3 丢弃vector

离开作用域自动丢弃



#### 2.4.4 读取元素

```rust
// 通过下标获取
let one:&i32 = &v[0];

// 使用 match 和 get 函数获取，推荐使用该方式，可以处理索引越界情况
match v.get(1) {
    Some(value) => println!("value = {}", value),
    _ => println!("do nothing"),
}
```



#### 2.4.5 更新

```rust
let mut v1: Vec<i32> = Vec::new();
v1.push(1);
v1.push(2);
v1.push(3);
```



#### 2.4.6 遍历

* ##### 不可变的遍历

  ```rust
  for i in &v1 {
      println!("i = {}", i);
  }
  ```

  

* ##### 可变的遍历

  ```rust
  for i in &mut v1 {
      *i += 1;
      println!("i = {}", i)
  }
  println!("v1 = {:?}", v1);
  ```

  

#### 2.4.7 使用枚举

通过使用枚举，在vector中存入不同类型的值

```rust
#[derive(Debug)]
enum Content {
    Text(String),
    Float(f32),
    Int(i32),
}
let c = vec![
    Content::Text(String::from("string")),
    Content::Int(-1),
    Content::Float(0.001),
];
println!("c = {:?}", c);
```



#### 2.4.8 补充

```rust
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0]; // 使用不可变引用
v.push(6); // 使用可变引用（借用）
// println!("first = {}", first); // 该行会报错。之前的不可变引用不能再使用，遵循上文引用与借用（可变引用）的使用规则
```



### 2.5 字符串(String、&str)

String 是栈上指向的堆上的一个地址，原理类似Vector

&str 是引用自“预分配文本(preallocated text)”的字符串切片，这个预分配文本存储在可执行程序的只读内存中。

#### 2.5.1 创建一个空 String

```rust
let mut s0 = String::new();
```



#### 2.5.2 通过字面值创建一个 String

使用字面值创建的是 &str

1. ##### 使用 String::from()

   ```rust
   let s1: String = String::from("init something");
   ```

   

2. ##### 使用 &str 的方式，&str 与 String 的转换

   ```rust
   // let s1: &str = "init something";
   let s1: String = "init something".to_string();
   ```

   

#### 2.5.3 更新 String

1. ##### push_str

   ```rust
   let mut s2 = String::from("hello");
   s2.push_str(" world");
   let ss = "!".to_string();
   s2.push_str(&ss);
   println!("s2 = {}", s2);
   ```

   

2. ##### push

   只能添加一个字符（char），可以是一个汉字：

   ```rust
   let mut s3 = String::from("tea");
   s3.push('m');
   println!("s3 = {}", s3);
   ```

   

3. ##### 使用  `+`  合并字符串

   ```rust
   let s4 = "hello".to_string();
   let s5 = String::from(" world");
   let s6 = s4 + &s5;
   println!("s6 = {}", s6);
   // println!("s4 = {}", s4); // s4 不能再继续使用了，s4 的所有权已经转移给 s6 了
   ```

   

4. ##### 使用 format

   ```rust
   let s7 = String::from("tic");
   let s8 = String::from("tac");
   let s9 = String::from("toe");
   // 使用 format 宏，按照指定格式合并字符串
   let s10 = format!("{}-{}-{}", s7, s8, s9);
   println!("s10 = {}", s10);
   // 依旧可以使用，宏不会转移所有权
   println!("s7 = {}", s7);
   println!("s8 = {}", s8);
   println!("s9 = {}", s9);
   ```

   

#### 2.5.4  String、&str 索引

* rust 中字符串采用 UTF-8 编码
* 不能直接通过索引获取



#### 2.5.5 遍历

1. ##### chars

   ```rust
   let s11 = String::from("你好");
   for c in s11.chars() {
       println!("c = {}", c);
   }
   println!("s11 = {}", s11);
   ```

   

2. ##### bytes

   打印出的都是字节的整数

   ```rust
   for b in s11.bytes() {
       println!("b = {}", b);
   }
   println!("s11 = {}", s11);
   ```

   

### 2.6 HashMap

#### 2.6.1 创建 HashMap

```rust
// 第一种方式
let mut scores: HashMap<String, i32> = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Red"), 20);
println!("scores = {:?}", scores);

// 第二种方式
let keys = vec![String::from("Blue"), String::from("Red")];
let values = vec![10, 20];
let scores: HashMap<&String, &i32> = keys.iter().zip(values.iter()).collect();
println!("scores = {:?}", scores);
```



#### 2.6.2 读取

```rust
let key = String::from("Blue");
let value = scores.get(&key); // get 返回的是一个Option
match value {
    Some(v) => println!("v = {}", v),
    None => println!("None"),
}
// if-let 语法，还有 while-let 语法
if let Some(v) = scores.get(&key) {
    println!("v = {}", v);
}
```



#### 2.6.3 遍历

```rust
for (key, value) in &scores {
    println!("{}, {}", key, value);
}
```



#### 2.6.4 更新

```rust
let mut map = HashMap::new();
map.insert(String::from("one"), 1);
map.insert(String::from("two"), 2);
map.insert(String::from("three"), 3);
map.insert(String::from("one"), 11); // 键相同会进行覆盖
map.entry(String::from("one")).or_insert(111); // 判断键不存在时，才会进行插入
println!("map = {:?}", map);
```

examp：（统计字符串中单词出现的个数）

```rust
let text = "hello world wonderful world";
let mut map:HashMap<&str, i32> = HashMap::new(); // 手动标注 HashMap 的泛型
for word in text.split_whitespace() {
    let count = map.entry(word).or_default(); // or_default 自动插入 value 对应类型的默认"零值"
    *count += 1;
}
println!("map = {:?}", map);

let text = "hello world wonderful world";
let mut map = HashMap::new(); // 根据下面插入值，自动推导 HashMap 泛型
for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0); // 插入0
    *count += 1;
}
println!("map = {:?}", map);
```

