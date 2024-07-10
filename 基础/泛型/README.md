# 泛型

```cj
class List<T> {
    var elem: Option<T> = None
    var tail: Option<List<T>> = None
}

func sumInt(a: List<Int64>) {  }
```