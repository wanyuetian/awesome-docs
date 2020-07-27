# 切片

![image](http://note.youdao.com/yws/res/78797/B5D24E47D2C54C33A32EBB77AA796ECB)

上图中一个切片变量包含三个域，分别是`底层数组的指针`、`切片的长度 length` 和`切片的容量 capacity`。切片支持 append 操作可以将新的内容追加到底层数组，也就是填充上面的灰色格子。如果格子满了，切片就需要扩容，底层的数组就会更换。

形象一点说，切片变量是底层数组的视图，底层数组是卧室，切片变量是卧室的窗户。通过窗户我们可以看见底层数组的一部分或全部。一个卧室可以有多个窗户，不同的窗户能看到卧室的不同部分。

![image](http://note.youdao.com/yws/res/102069/08737C5625A64C33AED993468624A0F4)

![image](http://note.youdao.com/yws/res/102068/5C5C52ADC357479CAD43453891F452CE)

![image](http://note.youdao.com/yws/res/102067/8ECADEF816DA4E669221A71DA8319F23)

## 切片的创建

切片的创建有多种方式，我们先看切片最通用的创建方法，那就是内置的 make 函数

```go
package main

import "fmt"

func main() {
 var s1 []int = make([]int, 5, 8)
 var s2 []int = make([]int, 8) // 满容切片
 fmt.Println(s1)
 fmt.Println(s2)
}

-------------
[0 0 0 0 0]
[0 0 0 0 0 0 0 0]
```

make 函数创建切片，需要提供三个参数，分别是切片的类型、切片的长度和容量。其中第三个参数是可选的，如果不提供第三个参数，那么长度和容量相等，也就是说切片的满容的。切片和普通变量一样，也可以使用类型自动推导，省去类型定义以及 var 关键字。比如上面的代码和下面的代码是等价的。

## 切片的初始化

```go
package main

import "fmt"

func main() {
 var s []int = []int{1,2,3,4,5}  // 满容的
 fmt.Println(s, len(s), cap(s))
}

---------
[1 2 3 4 5] 5 5
```

## 空切片

```go
package main

import "fmt"

func main() {
 var s1 []int
 var s2 []int = []int{}
 var s2 []int = make([]int, 0)
 fmt.Println(s1, s2, s3)
 fmt.Println(len(s1), len(s2), len(s3))
 fmt.Println(cap(s1), cap(s2), cap(s3))
}

-----------
[] [] []
0 0 0
0 0 0
```

## 切片的赋值

```go
package main

import "fmt"

func main() {
 var s1 = make([]int, 5, 8)
 // 切片的访问和数组差不多
 for i := 0; i < len(s1); i++ {
  s1[i] = i + 1
 }
 var s2 = s1
 fmt.Println(s1, len(s1), cap(s1))
 fmt.Println(s2, len(s2), cap(s2))

 // 尝试修改切片内容
 s2[0] = 255
 fmt.Println(s1)
 fmt.Println(s2)
}

--------
[1 2 3 4 5] 5 8
[1 2 3 4 5] 5 8
[255 2 3 4 5]
[255 2 3 4 5]
```

切片的赋值是一次浅拷贝操作，拷贝的是切片变量的三个域，你可以将切片变量看成长度为 3 的 int 型数组，数组的赋值就是浅拷贝。拷贝前后两个变量共享底层数组，对一个切片的修改会影响另一个切片的内容，这点需要特别注意。  
从上面的输出中可以看到赋值的两切片共享了底层数组。

## 切片的遍历

```go
package main


import "fmt"


func main() {
    var s = []int{1,2,3,4,5}
    for index := range s {
        fmt.Println(index, s[index])
    }
    for index, value := range s {
        fmt.Println(index, value)
    }
}

--------
0 1
1 2
2 3
3 4
4 5
0 1
1 2
2 3
3 4
4 5
```

## 切片的追加

```go
package main

import "fmt"

func main() {
 var s1 = []int{1,2,3,4,5}
 fmt.Println(s1, len(s1), cap(s1))

 // 对满容的切片进行追加会分离底层数组
 var s2 = append(s1, 6)
 fmt.Println(s1, len(s1), cap(s1))
 fmt.Println(s2, len(s2), cap(s2))

 // 对非满容的切片进行追加会共享底层数组
 var s3 = append(s2, 7)
 fmt.Println(s2, len(s2), cap(s2))
 fmt.Println(s3, len(s3), cap(s3))
}

--------------------------
[1 2 3 4 5] 5 5
[1 2 3 4 5] 5 5
[1 2 3 4 5 6] 6 10
[1 2 3 4 5 6] 6 10
[1 2 3 4 5 6 7] 7 10
```

文章开头提到切片是动态的数组，其长度是可以变化的。什么操作可以改变切片的长度呢，这个操作就是追加操作。切片每一次追加后都会形成新的切片变量，如果底层数组没有扩容，那么追加前后的两个切片变量共享底层数组，如果底层数组扩容了，那么追加前后的底层数组是分离的不共享的。如果底层数组是共享的，一个切片的内容变化就会影响到另一个切片，这点需要特别注意。

```go
package main

import "fmt"

func main() {
 var s1 = []int{1,2,3,4,5}
 append(s1, 6)
 fmt.Println(s1)
}

--------------
./main.go:7:8: append(s1, 6) evaluated but not used
```

正是因为切片追加后是新的切片变量，Go 编译器禁止追加了切片后不使用这个新的切片变量，以避免用户以为追加操作的返回值和原切片变量是同一个变量。

## 切片的域是只读的

我们刚才说切片的长度是可以变化的，为什么又说切片是只读的呢？这不是矛盾么。这是为了提醒读者注意切片追加后形成了一个新的切片变量，而老的切片变量的三个域其实并不会改变，改变的只是底层的数组。这里说的是切片的「域」是只读的，而不是说切片是只读的。切片的「域」就是组成切片变量的三个部分，分别是底层数组的指针、切片的长度和切片的容量。

## 切割

![image](http://note.youdao.com/yws/res/78847/F08AE3352B154B839D1597E8E518C521)

```go
package main

import "fmt"

func main() {
 var s1 = []int{1,2,3,4,5,6,7}
 // start_index 和 end_index，不包含 end_index
 // [start_index, end_index)
 var s2 = s1[2:5] 
 fmt.Println(s1, len(s1), cap(s1))
 fmt.Println(s2, len(s2), cap(s2))
}

------------
[1 2 3 4 5 6 7] 7 7
[3 4 5] 3 5
```

```go
package main

import "fmt"

func main() {
 var s1 = []int{1, 2, 3, 4, 5, 6, 7}
 var s2 = s1[:5]
 var s3 = s1[3:]
 var s4 = s1[:]
 fmt.Println(s1, len(s1), cap(s1))
 fmt.Println(s2, len(s2), cap(s2))
 fmt.Println(s3, len(s3), cap(s3))
 fmt.Println(s4, len(s4), cap(s4))
}

-----------
[1 2 3 4 5 6 7] 7 7
[1 2 3 4 5] 5 7
[4 5 6 7] 4 4
[1 2 3 4 5 6 7] 7 7
```

上面的 s1[:] 很特别，它和普通的切片赋值有区别么？答案是没区别，这非常让人感到意外，同样的共享底层数组，同样是浅拷贝。下面我们来验证一下

```go
package main

import "fmt"

func main() {
 var s = make([]int, 5, 8)
 for i:=0;i<len(s);i++ {
  s[i] = i+1
 }
 fmt.Println(s, len(s), cap(s))

 var s2 = s
 var s3 = s[:]
 fmt.Println(s2, len(s2), cap(s2))
 fmt.Println(s3, len(s3), cap(s3))

 // 修改母切片
 s[0] = 255
 fmt.Println(s, len(s), cap(s))
 fmt.Println(s2, len(s2), cap(s2))
 fmt.Println(s3, len(s3), cap(s3))
}

-------------
[1 2 3 4 5] 5 8
[1 2 3 4 5] 5 8
[1 2 3 4 5] 5 8
[255 2 3 4 5] 5 8
[255 2 3 4 5] 5 8
[255 2 3 4 5] 5 8
```

## 数组变切片

```go
package main

import "fmt"

func main() {
    var a = [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    var b = a[2:6]
    fmt.Println(b)
    a[4] = 100
    fmt.Println(b)
}

-------
[3 4 5 6]
[3 4 100 6]
```

## copy 函数

Go 语言还内置了一个 copy 函数，用来进行切片的深拷贝。不过其实也没那么深，只是深到底层的数组而已。如果数组里面装的是指针，比如 []*int 类型，那么指针指向的内容还是共享的。

```go
func copy(dst, src []T) int
```

copy 函数不会因为原切片和目标切片的长度问题而额外分配底层数组的内存，它只负责拷贝数组的内容，从原切片拷贝到目标切片，拷贝的量是原切片和目标切片长度的较小值 —— min(len(src), len(dst))，函数返回的是拷贝的实际长度。我们来看一个例子

```go
package main

import "fmt"

func main() {
 var s = make([]int, 5, 8)
 for i:=0;i<len(s);i++ {
  s[i] = i+1
 }
 fmt.Println(s)
 var d = make([]int, 2, 6)
 var n = copy(d, s)
 fmt.Println(n, d)
}
-----------
[1 2 3 4 5]
2 [1 2]
```

## 切片的扩容点

当比较短的切片扩容时，系统会多分配 100% 的空间，也就是说分配的数组容量是切片长度的2倍。但切片长度超过1024时，扩容策略调整为多分配 25% 的空间，这是为了避免空间的过多浪费。试试解释下面的运行结果。

```go
s1 := make([]int, 6)
s2 := make([]int, 1024)
s1 = append(s1, 1)
s2 = append(s2, 2)
fmt.Println(len(s1), cap(s1))
fmt.Println(len(s2), cap(s2))
-------------------------------------------
7 12
1025 1344
```
