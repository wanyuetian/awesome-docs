# 常量

## const

```go
package main

import "fmt"

const globali int = 24

func main() {
    const locali int = 42
    fmt.Println(globali, locali)
}
```

## iota

iota是go语言的常量计数器，只能在常量的表达式中使用。

iota在const关键字出现时将被重置为0。const中每新增一行常量声明将使iota计数一次(iota可理解为const语句块中的行索引)。 使用iota能简化定义，在定义枚举时很有用。

```go
const (
    n1 = iota //0
    n2        //1
    n3        //2
    n4        //3
)
```

使用_跳过某些值

```go
const (
    n1 = iota //0
    n2        //1
    _
    n4        //3
)
```

iota声明中间插队

```go
const (
    n1 = iota //0
    n2 = 100  //100
    n3 = iota //2
    n4        //3
)
const n5 = iota //0
```
定义数量级 

```go
const (
    _  = iota
    KB = 1 << (10 * iota)
    MB = 1 << (10 * iota)
    GB = 1 << (10 * iota)
    TB = 1 << (10 * iota)
    PB = 1 << (10 * iota)
)
```

多个iota定义在一行

```go
const (
    a, b = iota + 1, iota + 2 //1,2
    c, d                      //2,3
    e, f                      //3,4
)
```
