# 变量

## 定义变量

```go
package main

import "fmt"

func main() {
    // 方式一
    var s int = 42  
    fmt.Println(s)
    // 方式二
    var s = 42
    fmt.Println(s)
    // 方式三
    s := 42
    fmt.Println(s)
}
```

## 全局变量

```go
package main

import "fmt"

var globali int = 24  // 全局变量

func main() {
    fmt.Println(globali, locali)
}
```

## 局部变量

```go
package main

import "fmt"

func main() {
    var locali int = 42  // 局部变量
    fmt.Println(globali, locali)
}
```

## 匿名变量

在使用多重赋值时，如果想要忽略某个值，可以使用匿名变量。匿名变量用一个下划线`_`表示：

```go
func foo() (int, string) {
    return 10, "Q1mi"
}
func main() {
    x, _ := foo()
    _, y := foo()
    fmt.Println("x=", x)
    fmt.Println("y=", y)
}
```
