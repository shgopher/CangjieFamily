<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-07 17:11:17
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-10 17:17:23
 * @FilePath: /CangjieFamily/基础/类/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# class

class 与 struct 的主要区别在于：**class 是引用类型**，struct 是值类型，它们在赋值或传参时行为是不同的

lass 之间可以继承，但 struct 之间不能继承。

```cj
class Rectangle {
    let width: Int64
    let height: Int64

    public init(width: Int64, height: Int64) {
        this.width = width
        this.height = height
    }

    public func area() {
        width * height
    }
}
```

## class 总结器

因为class是引用类型，所以要想主动垃圾回收可以使用这个

```cj
Class C{
  var p:Int64

  init(p:Int64){
    this.p=p
  }

  ~init(){ // 固定用法 ～init()
    unsafe{
      LibC.free(p) // 释放内存 P
    }
  }
}
```

## 成员变量

```cj
class Rectangle {
    let width = 10 // 成员变量，两者都可以有初始值
    static let height = 20 // 静态成员变量 
 }
```
## 成员函数

```cj
class Rectangle {
    let width: Int64 = 10
    let height: Int64 = 20

    public func area() {
        this.width * this.height
    }

    public static func typeName(): String {
        "Rectangle"
    }
}

```
### 抽象成员函数
根据有没有函数体，实例成员函数又可以分为抽象成员函数和非抽象成员函数。

抽象成员函数没有函数体，只能定义在抽象类或接口

```cj
abstract class AbRectangle {
    public func foo(): Unit // 注意这里没有函数体
}
```

非抽象函数必须有函数体，在函数体中可以通过 this 访问实例成员变量

```cj
class Rectangle {
    let width: Int64 = 10
    let height: Int64 = 20

    public func area() {
        this.width * this.height
    }
}

```

需要注意的是，抽象实例成员函数默认具有 open 的语义，open 修饰符是可选的，且必须使用 public 或 protected 进行修饰。


## class 成员的可见修饰符

可以使用的可见性修饰符有三种：public、protected 和 private，**缺省的含义是仅包内可见 意思就是 protected**

- 使用 public 修饰的成员在 class 定义内部和外部均可见，成员变量、成员属性和成员函数在 class 外部可以通过对象访问；
- 使用 protected 修饰的成员在本包、本 class 及其子类中可见，外部无法访问；
- 使用 private 修饰的成员仅在本 class 定义内部可见，外部无法访问；缺省可见修饰符的成员仅在本包可见，外部无法访问。


当带 open 修饰的实例成员被 class 继承


```cj
open class A {
    let a: Int64 = 10
}

class B <: A { // Ok: 'B' Inheritance 'A'
    let b: Int64 = 20
}

class C <: B { // Error: 'B' is not inheritable
    let c: Int64 = 30
}

```

class 仅支持单继承(不过如果是接口的话，倒是可以同时实现多个接口)

sealed 修饰符只能修饰抽象类，表示被修饰的 class 定义只能在本定义所在的包内被其他 class 继承。sealed 已经蕴含了 public/open 的语义，因此定义 sealed abstract class 时若提供 public/open 修饰符，编译器将会告警


```cj
public sealed abstract class C1 {} 
```