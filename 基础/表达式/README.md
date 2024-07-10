<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-06-29 17:17:42
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-02 16:59:48
 * @FilePath: /CangjieFamily/基础/表达式/README.md
 * @Description
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 表达式
## if
```cj
if (a >1) {
  println("a > 1")
} else if (a == 1){
  println("a == 1")
} else {
  println("a < 1")
}
```
## while
```cj
var a = 100
while (a > 1) {
  println("YES!")
  a--
}
```
## do-while
```cj
var a = 100
do {
  println("YES!")
  a--
} while (a > 1)
```
## for
for-in 表达式可以遍历那些扩展了迭代器接口 Iterable<T> 的类型实例，仓颉内置的区间和数组等类型已经扩展了该接口

```cj
let myArray=[1,2,3,4,5]
for(v in myArray){
  println(v)
}
```
### where
for-in 表达式可以添加一个 where 子句，该子句会过滤掉不符合条件的元素。

```cj
 main() {
    for (i in 0..8 where i % 2 == 1) { // i 为奇数才会执行循环体
        println(i)
    }
}
```

当然 for-in 循环中还有 break 和 continue 语句，跟其它编程语言一样，不多赘述了

## 作用域
在仓颉编程语言中，用一对大括号 “{}” 包围一段仓颉代码，即构造了一个新的作用域


## 程序基本结构

顶层作用域中，变量和函数分别被称为**全局变量和全局函数**

在非顶层作用域中不能定义上述自定义类型，但可以定义变量和函数，称之为**局部变量和局部函数**

对于定义在自定义类型中的变量和函数，称之为**成员变量和成员函数**