<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-06-23 17:58:58
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-06-29 17:16:00
 * @FilePath: /CangjieFamily/基础/概述/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 概述
## 变量和常量
- 可变变量   var a: Int64 = 1
- 不可变变量 let b: Int64 = 2 
- 常量      const c: Int64 = 3

## 仓颉变量不具有初始值

在有些编程语言中，变量在声明的过程中就自动获得初始值（比如 Go）然而仓颉不是，仓颉的变量在声明的时候没有初始值，必须显式赋值，或者在变量被引用前赋值，不过，定义全局变量和静态成员变量时必须初始化。

```cj
main(){
  let a: Int64
  a = 1 
  println(a)
}
```
```cj
let a: Int64 = 123

class N {
  static let b: Int64 = 123
}
```
