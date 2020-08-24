# Rust

## 准备工作

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



## Rust 基础

### 通用编程概念

#### 变量（可类型推导）：let name: type;

* ##### 可变性

  定义变量用 let ，如果变量没有用 mut 修饰，默认是不可变的，let mut name: type;

* ##### 常量

  const name: type;

* ##### 隐藏

  定义同名变量时，新定义的变量会在之后的作用域内将旧的隐藏，旧的变量在其作用域内依旧有效



#### 数据类型

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
     let (x, y, z) = tup; // 元组拆解
     ```

   * 结构体

   * 枚举

3. ##### 字符串类型



#### 函数

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



#### 注释

* //



#### 控制流

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



### 所有权

#### 所有权

##### 堆和栈

##### 所有权规则

##### 变量作用域

