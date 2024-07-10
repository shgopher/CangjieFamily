# Collection 类型

|类型名称|	元素可变|	增删元素|	元素唯一性|	有序序列|
|:---:|:---:|:---:|:---:|:---:|
|Array<T>|	Y|	N|	N	|Y|
|ArrayList<T>	|Y|	Y|	N|	Y|
|HashSet<T>|	N	|Y|	Y	|N|
|HashMap<K, V>|	K: N, V: Y	|Y|	K: Y, V: N|	N|
## 数组 Array
```cj
var a :Array<Int> = [1,2,3,4,5]
var b=[1,2,3,4,5]
```
对数组更改值

`a[0] = 100`

对数组遍历
```cj
for(v in a){
  println(v)
}
```

使用 a.size 获取数组长度

Array 是引用类型，因此 Array 在作为表达式使用时不会拷贝副本，同一个 Array 实例的所有引用都会共享同样的数据。

### 数组切片
let arr1 = [0，1，2，3，4，5，6]
let arr2 = arr1[..3]
## ArrayList
跟 go 语言一样的切片就是 arrayList，所以在仓颉中没有 go 中值类型数组这个概念，都是切片，一般的 array 是引用类型，但是不能扩容的切片，arrayList 是拥有扩容能力的切片

ArrayList 不是默认类型，所以要使用导入标准库的方式去导入
```cj
from std import collection.*

var a = ArrayList<Int64>([0,1,2])
a.append(0) // 使用append的方法去append数据
a.insert(1,3) // 0 3 2 使用inser去替换数据
a.remove(1) // 0 2  使用 remove去删除数据
```
## iterator() 接口

Array、ArrayList、HashSet、HashMap 类型都实现了 Iterable，因此我们都可以将其用在 for-in 或者 while-let 中。

```cj
var list = [1, 2, 3, 4, 5]
var it = list.iterator()
while (true){
  match (it.next()) {
    case (Some(v)) => println(v) 
    case (None) => break
    
  }
}

let list = [1, 2, 3]
var it = list.iterator()
while (let Some(i) <- it.next()) {
    println(i)
}

```
## hashset
我们可以使用 HashSet 类型来构造只拥有不重复元素的 Collection。其实它就是等于只取 hashmap 的 key 来用而已
```cangjie
form std import collection.*

let c = HashSet<Int64>([0, 1, 2])
```
如果需要将单个元素添加到 HashSet，请使用 put 函数。如果希望同时添加多个元素，可以使用 putAll 函数

```cj
let c = HashSet<String>()
c.put("hi")
c.putAll(["d","t"])
c.remove("d")
```
## hashmap
```cj
from std import collection.*

let b = HashMap<String, Int64>([("a", 0), ("b", 1), ("c", 2)])

for (k, v) in b { // range 遍历
  println(k, v)
}
println(b.size) // 打印数量

b.contains("a") // 判断是否包含

println(b["a"])
b["a"] = 12 // 更改数据

b.put("d", 3) // 添加单个数据

let map2 = HashMap<String, Int64>([("c", 2), ("d", 3)])
map.putAll(map2)
b.remove("d")
```
