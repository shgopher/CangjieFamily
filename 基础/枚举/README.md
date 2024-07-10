<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-07 17:10:46
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-10 14:45:05
 * @FilePath: /CangjieFamily/基础/枚举/README.md
 * @Descrip
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# 枚举类型
```cj

enum RGBColor {
    | Red | Green | Blue
}

```

在 enum 体中还可以定义一系列成员函数、操作符函数和成员属性
```cj
enum RGBColor {
    | Red | Green | Blue

    public static func printType() {
        print("RGBColor")
    }
}

``

```cj
enum RGBColor {
    | Red | Green | Blue(UInt8)
}

main() {
    let r = RGBColor.Red
    let g = Green
    let b = Blue(100)
}
```
## 模式匹配

```cj
enum RGBColor {
    | Red | Green | Blue
}

main() {
    let c = Green
    let cs = match (c) {
        case Red => "Red"
        case Green => "Green"  // Matched
        case Blue => "Blue"
    }
    print(cs)
}

```

## Option 类型

```cj
enum Option<T> {
    | Some(T)
    | None
}
```

Option 类型使用 enum 定义，它包含两个构造器：Some 和 None。其中，Some 会携带一个参数，表示有值，None 不带参数，表示无值


**当需要表示某个类型可能有值，也可能没有值的时候，可选择使用 Option 类型。**

Option 类型还有一种简单的写法：在类型名前加？
```cj
let a: Option<Int64> = Some(100)
let b: ?Int64 = Some(100)
let c: Option<String> = Some("Hello")
let d: ?String = None
```
