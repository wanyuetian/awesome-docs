# 流程控制

## if else 语句
```go
package main

import "fmt"

func main() {
    fmt.Println(sign(max(min(24, 42), max(24, 42))))
}

func max(a int, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a int, b int) int {
    if a < b {
        return a
    }
    return b
}

func sign(a int) int {
    if a > 0 {
        return 1
    } else if a < 0 {
        return -1
    } else {
        return 0
    }
}
```

## switch 语句
```go
package main

import "fmt"

func main() {
    fmt.Println(prize1(60))
    fmt.Println(prize2(60))
}

// 值匹配
func prize1(score int) string {
    switch score / 10 {
    case 0, 1, 2, 3, 4, 5:
        return "差"
    case 6, 7:
        return "及格"
    case 8:
        return "良"
    default:
        return "优"
    }
}

// 表达式匹配
func prize2(score int) string {
    // 注意 switch 后面什么也没有
    switch {
        case score < 60:
            return "差"
        case score < 80:
            return "及格"
        case score < 90:
            return "良"
        default:
            return "优"
    }
}
```
## for 循环
**死循环**
```go
package main

import "fmt"

func main() {
    for {
        fmt.Println("hello world!")
    }
}
```
**普通循环**
```go
package main

import "fmt"

func main() {
    for i := 0; i < 10; i++ {
        fmt.Println("hello world!")
    }
}
```