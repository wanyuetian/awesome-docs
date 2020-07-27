## 变量 {docsify-ignore-all}

### 定义变量
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
### 全局变量

```go
package main

import "fmt"

var globali int = 24  // 全局变量

func main() {
    fmt.Println(globali, locali)
}
```

### 局部变量

```go
package main

import "fmt"

func main() {
    var locali int = 42  // 局部变量
    fmt.Println(globali, locali)
}
```