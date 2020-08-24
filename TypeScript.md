# TypeScript

## 1. 静态类型

### 1.1 基础静态类型

#### Boolean

#### Number

#### String

#### Array

#### Tuple



#### Enum

```typescript
// 默认从0开始
enum Direction {
    Up = 0,
    Down,
    Left,
    Right
}

// 也可以指定
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "Right"
}

let dirc: Direction = Direction.Up;
```





#### Any

当使用第三方的类库的时候，不确定参数的类型，可以使用any类型。



#### Void

#### Null and Undifined

#### Never

#### Object



### 1.2 对象静态类型

对象

数组

类

函数



### 1.3 联合类型

当一个变量可能为多种类型时，使用联合类型

##### example:

```typescript
let numOrStr: number | string;
```



### 1.4 类型注解（type annotation）

不使用类型注解，默认类型为any。使用类型注解后，可以点出相对应类型的方法



### 1.5 类型推断（type inference）