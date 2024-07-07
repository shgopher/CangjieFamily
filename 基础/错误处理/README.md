<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-03 17:32:03
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-03 22:26:14
 * @FilePath: /CangjieFamily/基础/错误处理/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 错误处理

- Error  如果出现内部错误，只能通知给用户，尽量安全终止程序。
- Exception 描述的是程序运行时的逻辑错误或者 IO 错误导致的异常，例如数组越界或者试图打开一个不存在的文件等，这类异常需要在程序中捕获处理。

用户无法直接继承 Error 和它的子类，只能继承 Exception 和它的子类，

```cj
open class people <: Exception {
  public open func say() {
    println("hello")
  }
}
```
## 异常创建
由于异常是 class 类型，只需要按 class 对象的构建方式去创建异常即可。如表达式 people() 即创建了一个类型为 FatherException 的异常。

仓颉语言提供 throw 关键字，用于抛出异常。用 throw 来抛出异常时，throw 之后的表达式必须是 Exception 的子类型
## 异常处理

使用 try - catch - throw 语句来处理异常。

```cj
main() {
    try {
        throw people("I am an Exception!")
    } catch (e: people) {
        println(e)
        println("people is caught!")
    }finally {
    println(" 无论如何都要输出")
    }
}
```
## try -cath 用语用作普通类型

```cj
main() {
  let x = try {
    E()
  } catch(e: Exception){
    println(e)
  }
}
```
## Try-with-resources 表达式

Try-with-resources 表达式主要是为了自动释放非内存资源。

```cj
class R <: Resource {
    public func isClosed(): Bool {
        true
    }
    public func close(): Unit {
        print("R is closed")
    }
}

main() {
    try (r = R()) {
        println("Get the resource")
    }
}
```
## 异常处理的类型模式
```cj
main(): Int64 {
    try {
        throw IllegalArgumentException("This is an Exception!")
    } catch (e: OverflowException) { // 方法一
        println(e.message)
        println("OverflowException is caught!")
    } catch (e: IllegalArgumentException | NegativeArraySizeException) { // 方法二
        println(e.message)
        println("IllegalArgumentException or NegativeArraySizeException is caught!")
    } finally {
        println("finally is executed!")
    }
    return 0
}
```

- 方法一中是 捕获 OverflowException 类型以及其子类的异常

- 通过 ｜ 符号，表示或的意思，表示适配两种异常类以及他们的子类型


```cj
open class Father <: Exception {
    var father: Int32 = 0
}

class ChildOne <: Father {
    var childOne: Int32 = 1
}

class ChildTwo <: Father {
    var childTwo: Int32 = 2
}

main() {
    try {
        throw ChildOne()
    } catch (e: ChildTwo | ChildOne) { // 这里换成 e: Father 也可以
        println("ChildTwo or ChildOne?")
    }

```



## 异常处理的通配符模式

```cj
main() {
    try {
        throw ChildOne()
    } catch (_) { // 你看这个如果使用通配符_  它可以捕获同级 try 块内抛出的任意类型的异常，等价于类型模式中的 e: Exception，即捕获 Exception 子类所定义的异常
        println("ChildTwo or ChildOne?")
    
```

## Option类型用在错误处理上

Option类型是一个num类型

Option类型值得是 可选类型 ，就是这样的 `func getString(p: ?Int64): String{` p 就是

如果函数 getOrThrow 的参数值等于 Some(v) 则将 v 的值返回，如果参数值等于 None 则抛出异常

```cj
func getOrThrow(a: ?Int64) { // 可选参数
    match (a) {
        case Some(v) => v
        case None => throw NoneValueException()
    }
}
```
### 模式匹配

```cj
func getString(p: ?Int64): String{
    match (p) {
        case Some(x) => "${x}" // 如果参数值等于 Some(v) 则将 v 的值返回
        case None => "none" // 如果没有值就返回None
    }
}
main() {
    let a = Some(1)
    let b: ?Int64 = None
    let r1 = getString(a)
    let r2 = getString(b)
    println(r1)
    println(r2)

```




### getOrThrow 函数


对于 ?T 类型的表达式 e，可以通过调用 getOrThrow 函数实现解构。当 e 的值等于 Some(v) 时，getOrThrow() 返回 v 的值，否则抛出异常。举例如下：
```cj
main() {
    let a = Some(1)
    let b: ?Int64 = None
    let r1 = a.getOrThrow()
    println(r1)
    try {
        let r2 = b.getOrThrow()
    } catch (e: NoneValueException) {
        println("b is None")
    }
}
```
### coalescing 操作符(??)
coalescing 操作符（??）：对于 ?T 类型的表达式 e1，如果希望 e1 的值等于 None 时同样返回一个 T 类型的值 e2，可以使用 ?? 操作符。对于表达式 e1 ?? e2，当 e1 的值等于 Some(v) 时返回 v 的值，否则返回 e2 的值。


```cj
main() {
    let a = Some(1)
    let b: ?Int64 = None
    let r1: Int64 = a ?? 0
    let r2: Int64 = b ?? 0
    println(r1)
    println(r2)
}
```
### 问号操作符(?)


对于 ?T1 类型的表达式 e，当 e 的值等于 Some(v) 时，e?.b 的值等于 Option<T2>.Some(v.b)，否则 e?.b 的值等于 Option<T2>.None，其中 T2 是 v.b 的类型


```cj
struct R {
    public var a: Int64
    public init(a: Int64) {
        this.a = a
    }
}

let r = R(100)
let x = Some(r)
let y = Option<R>.None
let r1 = x?.a   // r1 = Option<Int64>.Some(100)
let r2 = y?.a   // r2 = Option<Int64>.None
```