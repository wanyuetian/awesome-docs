### 常量

#### const
```go
package main

import "fmt"

const globali int = 24

func main() {
    const locali int = 42
    fmt.Println(globali, locali)
}
```