<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-03 22:26:48
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-07 17:06:47
 * @FilePath: /CangjieFamily/基础/函数/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 函数
仓颉中的函数拥有自己的类型，举个例子
```cj
func get(a:Int64,b:Int64):Int64{
  return a+b
}
```
这个函数的类型名称就是 `(Int64,Int64) -> Int64`，所以你还可以这么写这个 get 函数

```cj
var get: (a:Int64,b:Int64) -> Int64 = { a:Int64,b:Int64 => a+b }// 函数类型 + lambda 表达式
```

函数参数均为不可变变量，在函数定义内不能对其赋值。

```cj
func add(a: Int64, b: Int64): Int64 {
    a = a + b // error
    return a
}
```

在函数定义时如果未显式定义返回值类型，编译器将根据函数体的类型以及函数体中所有的 return 表达式来共同推导出函数的返回值类型

```cj
func add(a: Int64, b: Int64) {
  return a+b
}
```

我们上述使用的是非命名参数，cj 有一种命名参数

```cj
func get(a!:Int64, b:!Int64){}

get(a:1,b:2)
```
命名参数可以拥有默认值，当然非命名参数就不能有了，而且命名参数的函数调用必须使用 key：value 的形式，一个函数中可以同有命名参数和非命名参数，但是命名参数必须放在非命名参数后面，否则编译器会报错

```cj
func get(a:Int64, b!:Int64=1){}
```

cj 中的函数可以不写返回的类型，让编译器自动推断即可

```cj
func add(a: Int64, b: Int64) {
  return a+b
}
```
如果没有返回值的时候，可以返回值写上 Uinit 类型

```cj
func add(a: Int64, b: Int64): Uinit {
  println(a+b)
  return
}
```

在 cj 中，我们知道命名变量的调用是 k-v 结构的，所以当出现多个命名参数时，可以传参顺序和定义顺序不同

于此同时，我们知道只有命名变量可以拥有默认值，所以当不传入这个拥有默认参数的命名变量时，就自动启用默认参数即可，不过当你传入拥有默认参数的命名变量时，**你传入的值会覆盖默认值**

## 函数是一等公民

我们在上文提到过，函数拥有类型，使用 `(参数类型,参数类型) -> 返回值类型` 例如 `(Int64,Int64) -> Int64` 来表示函数的类型，所以当函数作为普通类型去使用的时候可以作为参数，返回类型，变量

### 函数类型的类型参数
```cj
let f: (Int64,Int64) -> Int64 = { a,b => a+b } // 因为前面已经声明了变量的类型，所以在后面的lamda函数中就不用给a b 写上类型了
```

另外对于一个函数类型，只允许统一写类型参数名，或者统一不写类型参数名，不能交替存在。

`let handler: (name: String, Int64) -> Int64   // error`

### 函数类型作为返回类型
```cj
func returnAdd(): (Int64, Int64) -> Int64 { // 返回类型是一个函数类型
    add
}
```
### 函数类型作为变量类型
```cj
func add(p1: Int64, p2: Int64): Int64 {
    p1 + p2
}

let f: (Int64, Int64) -> Int64 = add
```
```cj
func printAdd(add: (Int64, Int64) -> Int64, a: Int64, b: Int64): Unit { // add 是一个函数类型
    println(add(a, b))
}

```

## 嵌套函数

定义在源文件顶层的函数叫做全局函数，定义在函数体内部的函数称之为嵌套函数

>(在 go 语言中，只能定义匿名函数在函数体内部)
```go
func getd() func() {

	var age = func() {

		fmt.Println("hi")
	}
	return age
}

func main() {getd()()
}
```
这是cj的嵌套函数
```cj
func getd() {
  func age() { // 这种写法go是不允许的，go只能匿名函数在函数体内部
    println("hi")
  }
  return age
}
``
## lambda 表达式

{a:Int64,b:Int64 => a+b} 

lambda 表达式 必须使用大括号包裹，前面是参数列表，后面是函数体，中间使用 => 符号去链接，如果前面写出来了函数类型，参数可以不写类型

```cj
let age: (Int64, Int64) -> Int64 = { a, b => a + b } // 后面省略了return
```

lambda 不可省略=> 符合，即便前面没有参数 `{=> println("d") }`

## 闭包

闭包是嵌入函数以及母函数中被嵌入函数引用用的变量所共同组成的结构叫做闭包

```cj
func agt(): () -> Unit {
  var a :int = 1
  func age(){
    println(a)
    a++
  }
}

main(){
  var age = agt()
  age() //1
  age() //2
}
```
## 函数调用语法糖
### 尾调 lambda
```cj
func myIf(a: Bool, fn: () -> Int64) {
    if(a) {
        fn()
    } else {
        0
    }
}
```
```cj
myIf(true) {        // Trailing closure call
        100
    }
```

当函数调用有且只有一个 lambda 实参时，我们还可以省略 ()，只写 lambda。
```cj
func f(fn: (Int64) -> Int64) { fn(1) }
```
```cj
func test() {
    f { i => i * i }
}
```

### Flow 表达式
`e1 |> e2 ` 等价于 `let v = e1 ; e2(v)`

```cj
func add(a: Array<Int64>): Array<Int64> {
}
func sum(a: Array<Int64>): Int64 {
}

let arr : Array<Int64> = [1, 2, 3, 4, 5]

let value = arr |> add |> sum
```
### Composition 表达式
`f ~> g`

`{ x => g(f(x)) } `

```cj
func f(x: Int64): Float64 {
    Float64(x)
}
func g(x: Float64): Float64 {
    x
}

var fg = f ~> g // The same as { x: Int64 => g(f(x)) }

```
## 变长参数

当参数最后一个参数是 array 的时候就可以变成变长形参
```cj
func add(x:Array<Int64>) {

}

add(1,2,3,4,5)
```

## 函数重载
```cj
func age(){

}

func age(a:Int64){

}
```

这属于两个函数，这就叫做函数重载，go 语言不支持
## 操作符重载
如果希望在某个类型上支持此类型默认不支持的操作符，可以使用操作符重载实现。

操作符函数定义与普通函数定义相似，区别如下：

- 定义操作符函数时需要在 func 关键字前面添加 operator 修饰符；
- 操作符函数的参数个数需要匹配对应操作符的要求
- 操作符函数只能定义在 class、interface、struct、enum 和 extend 中；
- 操作符函数具有实例成员函数的语义，所以禁止使用 static 修饰符；
- 操作符函数不能为泛型函数。


## Mut 函数

Struct 类型是值类型，其实例成员函数无法修改实例本身。例如，下例中，成员函数 g 中不能修改成员变量 i 的值。

Mut 函数与普通的实例成员函数相比，多一个 mut 关键字来修饰。

Mut 函数是一种可以修改 struct 实例本身的特殊的实例成员函数。在 mut 函数内部，this 的语义是特殊的，这种 this 拥有原地修改字段的能力。

只允许在 interface、struct 和 struct 的扩展内定义 mut 函数

```cj
struct Foo {
    var i = 0

    public mut func g() {
        i += 1  // ok
    }
}
```




