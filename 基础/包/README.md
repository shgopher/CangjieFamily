<!--
 * @Author: shgopher shgopher@gmail.com
 * @Date: 2024-07-11 00:24:33
 * @LastEditors: shgopher shgopher@gmail.com
 * @LastEditTime: 2024-07-11 00:38:07
 * @FilePath: /CangjieFamily/基础/包/README.md
 * @Description: 
 * 
 * Copyright (c) 2024 by shgopher, All Rights Reserved. 
-->
# cangjie 包

## 包的名字

```cj
package test
```

```bash
src
`-- directory_0
    |-- directory_1
    |    |-- a.cj
    |    `-- b.cj
    `-- c.cj
`-- main.cj
```

```cj
// a.cj
package directory_0.directory_1
```
```cj
// b.cj
package directory_0.directory_1

```
```cj
// c.cj
package directory_0
```

```cj
// main 包不需要写 package main
main() {
}
```


包声明不能引起命名冲突：
- 包名与当前包的顶层声明不能重名。
- 当前包的顶层声明不能与子目录的包名同名。

> go 语言没有这个限制

如果想导出外包需要加上 public 这个修饰符

## 导入包

from moduleName import packageName.itemName

moduleName 为模块名，packageName 为包名 itemName 为声明的名字。

导入当前模块中的内容时，可以省略 from moduleName；跨模块导入时，必须使用 from moduleName 指定模块

导入多个 item
```cj
from module_name import package1.{foo, bar, fuzz}
```

import packageName.* 语法将 packageName 包中所有 public 修饰的顶层声明或定义全部导入。

## 包名重命名

from module1 import p1。*as A。*使用 as 即可

## 程序入口

当一个包被导入时，包中定义的 main 不会被导入。
