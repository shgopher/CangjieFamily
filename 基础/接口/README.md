<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-07 17:11:28
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-10 18:05:50
 * @FilePath: /CangjieFamily/基础/接口/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 接口

接口的成员可以包含：

- 成员函数
- 操作符重载函数
- 成员属性

```cj
interface I {
    func f(): Unit
}
```
interface 默认具有 open 语义,因为默认interface 默认可以被实现


继承这个接口

```cj
class A <: I {
  public func f(): Unit {
    println("A")
  }
}

main(){
  let a = A()
  let b:I = a
  b.f()
}
```

## 接口继承

```cj
interface Addable {
    func add(other: Int64): Int64
}

interface Subtractable {
    func sub(other: Int64): Int64
}

interface Calculable <: Addable & Subtractable {
    func mul(other: Int64): Int64
    func div(other: Int64): Int64
}

```

## any类型

这个跟go一样

interface any {}  就等于 go中的 `type any = interface{}` ，any是空接口的别名

## 类型别名

跟go的一样 type aaa = Int64 ，不过看来cj中没有以xx为底的新类型这种方案



