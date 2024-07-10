<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-07 17:10:33
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-10 14:16:39
 * @FilePath: /CangjieFamily/基础/结构体/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 结构体

先看一个例子

```cj
struct Point {
  let width: Int64 // 成员变量
  static let height: Int64 //静态成员变量。一个 struct 中最多允许定义一个静态初始化器，否则报重定义错误。

  public init(width: Int64, height: Int64){ // 构造函数
    this.width = width
    this.height = height
  }

  public func area(){
    return width * height
  }
}
``` 


## struct 静态初始化器
一个 struct 中只有一个静态初始化器

```cj
struct Point {
  static let height: Int64
  static init(){
    degree = 100
  }
}
```
## struct 构造函数

```cj
struct Rectangle {
    let width: Int64
    let height: Int64

    public init(width: Int64, height: Int64) {  // 所有成员变量都要初始化
        this.width = width
        this.height = height
    }
}
```

一个 struct 中可以定义多个普通构造函数，但它们必须构成重载

```cj
struct Rectangle {
    let width: Int64
    let height: Int64

    public init(width: Int64) {
        this.width = width
        this.height = width
    }

    public init(width: Int64, height: Int64) { // Ok: overloading with the first init function
        this.width = width
        this.height = height
    }

    public init(height: Int64) { // Error: 不构成重载
        this.width = height
        this.height = height
    }

```

### 主构造函数

除了可以定义若干普通的以 init 为名字的构造函数外，struct 内还可以定义 (最多) **一个主构造函数**。主构造函数的名字和 struct 类型名相同

```cj
struct Rectangle {
  public Rectangle(let width: Int64, let height: Int64) {
    // 不用再写 this.width = width ,这是一个语法糖
  }
}
```
主构造函数的参数列表中也可以定义普通形参，例如：

```cj
struct Rectangle {
    public Rectangle(name: String, let width: Int64, let height: Int64) {} // name 就是普通形参
}

```
### 无参构造函数

如果 struct 定义中不存在自定义构造函数 (包括主构造函数)，并且所有实例成员变量都有初始值，则会自动为其生成一个无参构造函数

```cj
struct Rectangle {
    let width: Int64 = 10
    let height: Int64 = 10
    
    /* 自动生成的，并且隐藏的
    
    public init() {
    }
    
    */
}
```
## 成员函数
### 事例成员函数
实例成员函数只能通过 struct 实例访问

实例成员函数中可以访问静态成员变量以及静态成员函数。
```cj
struct Rectangle {
    let width: Int64 = 10
    let height: Int64 = 20

    public func area() { // 成员函数
        this.width * this.height // 成员函数通过this访问事例成员变量
    }

    public static func typeName(): String { // 静态成员函数
        "Rectangle"
    }
}
```
### 静态成员函数
静态成员函数只能通过 struct 类型名访问

静态成员函数中不能访问实例成员变量，也不能调用实例成员函数

## struct 可用修饰符

struct 的成员 (包括成员变量、成员属性、构造函数、成员函数、操作符函数 (详见操作符重载章节)) 用两种可见性修饰符修饰：public 和 private，缺省的含义是仅包内可见

使用 public 修饰的成员在 struct 定义内部和外部均可见；使用 private 修饰的成员仅在 struct 定义内部可见，外部无法访问。

## struct 不可递归

```cj
//递归和互递归定义的 struct 均是非法的

struct R1 { // Error: 'R1' recursively references itself
    let other: R1
}
struct R2 { // Error: 'R2' and 'R3' are mutually recursive
    let other: R3
}
struct R3 { // Error: 'R2' and 'R3' are mutually recursive
    let other: R2
}

```
## 使用 struct 事例函数

在赋值或传参时，会对 struct 实例进行复制，生成新的实例，对其中一个实例的修改并不会影响另外一个实例。以赋值为例，下面的例子中，将 r1 赋值给 r2 之后，修改 r1 的 width 和 height 的值，并不会影响 r2 的 width 和 height 值。

```cj
struct Rectangle {
    public var width: Int64
    public var height: Int64

    public init(width: Int64, height: Int64) {
        this.width = width
        this.height = height
    }

    public func area() {
        width * height
    }
}

main() {
    var r1 = Rectangle(10, 20) // r1.width = 10, r1.height = 20
    var r2 = r1                // r2.width = 10, r2.height = 20
    r1.width = 8               // r1.width = 8
    r1.height = 24             // r1.height = 24
    let a1 = r1.area()         // a1 = 192
    let a2 = r2.area()         // a2 = 200
}

```
## mut 函数

默认情况下，struct 中的实例成员函数是无法修改它的实例成员变量和实例成员属性的。如果需要修改，可以在定义实例成员函数时使用关键字 mut 修饰，

```cj
struct Rectangle {
    public var width: Int64
    public var height: Int64

    public init(width!: Int64, height!: Int64) {
        this.width = width
        this.height = height
    }

    public mut func setW(v: Int64): Unit {
        width = v
    }

    public func area() {
        width * height
    }
}

```

调用 mut 函数时，mut 函数允许修改当前实例的成员值，例如，下例中通过调用 setW 将 r 的 width 值设置为 8。
```cj
main() {
    var r = Rectangle(width: 10, height: 20) // r.width = 10, r.height = 20
    r.setW(8)                 // r.width = 8
    let a = r.area()          // a = 160
}
```
注意，上例中 r 如果使用 let 定义，则不能调用 Rectangle 中的 mut 函数。