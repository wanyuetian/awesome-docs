# 映射

数组切片让我们具备了可以操作一块连续内存的能力，它是对同质元素的统一管理。而映射则赋予了不连续不同类的内存变量的关联性，它表达的是一种因果关系，映射的 key 是因，映射的 value 是果。如果说数组和切片赋予了我们步行的能力，那么映射则让我们具备了跳跃的能力。

指针、数组切片和映射都是容器型变量，映射比数组切片在使用上要简单很多，但是内部结构却无比复杂。

## 映射的创建

```go
package main

import "fmt"

func main() {
    var m map[int]string = make(map[int]string)
    fmt.Println(m, len(m))
}

----------
map[] 0
```

使用 make 函数创建的映射是空的，长度为零，内部没有任何元素。如果需要给映射提供初始化的元素，就需要使用另一种创建映射的方式。

```go
package main

import "fmt"

func main() {
    var m map[int]string = map[int]string{
        90: "优秀",
        80: "良好",
        60: "及格",  // 注意这里逗号不可缺少，否则会报语法错误
    }
    fmt.Println(m, len(m))
}

---------------
map[90:优秀 80:良好 60:及格] 3
```

如果你可以预知映射内部键值对的数量，那么还可以给 make 函数传递一个整数值，通知运行时提前分配好相应的内存。这样可以避免映射在长大的过程中要经历的多次扩容操作。

```go
var m = make(map[int]string, 16)
```

## 映射的读写

```go
package main

import "fmt"

func main() {
    var fruits = map[string]int {
        "apple": 2,
        "banana": 5,
        "orange": 8,
    }
    // 读取元素
 var score = fruits["banana"]
    fmt.Println(score)

 // 增加或修改元素
    fruits["pear"] = 3
    fmt.Println(fruits)

 // 删除元素
    delete(fruits, "pear")
    fmt.Println(fruits)
}

-----------------------
5
map[apple:2 banana:5 orange:8 pear:3]
map[orange:8 apple:2 banana:5]
```

## 映射 key 不存在会怎样

```go
package main

import "fmt"

func main() {
    var fruits = map[string]int {
        "apple": 2,
        "banana": 5,
        "orange": 8,
    }

    var score, ok = fruits["durin"]
    if ok {
        fmt.Println(score)
    } else {
        fmt.Println("durin not exists")
    }

    fruits["durin"] = 0
    score, ok = fruits["durin"]
    if ok {
        fmt.Println(score)
    } else {
        fmt.Println("durin still not exists")
    }
}

-------------
durin not exists
0
```

## 映射的遍历

```go
package main

import "fmt"

func main() {
    var fruits = map[string]int {
        "apple": 2,
        "banana": 5,
        "orange": 8,
    }

    for name, score := range fruits {
        fmt.Println(name, score)
    }

    for name := range fruits {
        fmt.Println(name)
    }
}

------------
orange 8
apple 2
banana 5
apple
banana
orange
```

奇怪的是，Go 语言的映射没有提供诸于 keys() 和 values() 这样的方法，意味着如果你要获取 key 列表，就得自己循环一下，如下

```go
package main

import "fmt"

func main() {
    var fruits = map[string]int {
        "apple": 2,
        "banana": 5,
        "orange": 8,
    }

    var names = make([]string, 0, len(fruits))
    var scores = make([]int, 0, len(fruits))

    for name, score := range fruits {
        names = append(names, name)
        scores = append(scores, score)
    }

    fmt.Println(names, scores)
}

----------
[apple banana orange] [2 5 8]
```
