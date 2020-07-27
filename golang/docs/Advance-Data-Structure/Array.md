# 数组

## 数组的定义

```go
package main

import "fmt"

func main() {
    var a [9]int
    fmt.Println(a)
}
```

```go
package main

import "fmt"

func main() {
    var a = [9]int{1, 2, 3, 4, 5, 6, 7, 8, 9}
    var b [10]int = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    c := [8]int{1, 2, 3, 4, 5, 6, 7, 8}
    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
}
```

## 数组的访问

```go
package main

import "fmt"

func main() {
    var squares [9]int
    for i := 0; i < len(squares); i++ {
        squares[i] = (i + 1) * (i + 1)
    }
    fmt.Println(squares)
}
```

## 数组赋值

```go
package main

import "fmt"

func main() {
    var a = [9]int{1, 2, 3, 4, 5, 6, 7, 8, 9}
    var b [9]int
    b = a
    a[0] = 12345
    fmt.Println(a)
    fmt.Println(b)
}
```

不同长度的数组之间赋值是禁止的，因为它们属于不同的类型

```go
package main

import "fmt"

func main() {
    var a = [9]int{1, 2, 3, 4, 5, 6, 7, 8, 9}
    var b [10]int
    b = a
    fmt.Println(b)
}

--------------------------
./main.go:8:4: cannot use a (type [9]int) as type [10]int in assignment
```

## 数组的遍历

```go
package main

import "fmt"

func main() {
    var a = [5]int{1,2,3,4,5}
    for index := range a {
        fmt.Println(index, a[index])
    }
    for index, value := range a {
        fmt.Println(index, value)
    }
}
```
