# 基础语法

- [变量](#变量)
- [基本数据类型](#基本数据类型)
- [流程控制](#流程控制)


## 变量

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

#### iota

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
```
const (
	n1 = iota //0
	n2 = 100  //100
	n3 = iota //2
	n4        //3
)
const n5 = iota //0
```
定义数量级 

```
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

```
const (
	a, b = iota + 1, iota + 2 //1,2
	c, d                      //2,3
	e, f                      //3,4
)
```


### 匿名变量

在使用多重赋值时，如果想要忽略某个值，可以使用匿名变量。匿名变量用一个下划线`_`表示：

```
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
## 基本数据类型
```go
package main

import "fmt"

func main() {
    // 有符号整数，可以表示正负
    var a int8 = 1 // 1 字节
    var b int16 = 2 // 2 字节
    var c int32 = 3 // 4 字节
    var d int64 = 4 // 8 字节
    fmt.Println(a, b, c, d)

    // 无符号整数，只能表示非负数
    var ua uint8 = 1
    var ub uint16 = 2
    var uc uint32 = 3
    var ud uint64 = 4
    fmt.Println(ua, ub, uc, ud)

    // int 类型，在32位机器上占4个字节，在64位机器上占8个字节
    var e int = 5
    var ue uint = 5
    fmt.Println(e, ue)

    // bool 类型
    var f bool = true
    fmt.Println(f)

    // 字节类型
    var j byte = 'a'
    fmt.Println(j)

    // 字符串类型
    var g string = "abcdefg"
    fmt.Println(g)

    // 浮点数
    var h float32 = 3.14
    var i float64 = 3.141592653
    fmt.Println(h, i)
}
```

## 流程控制

### if else 语句
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

### switch 语句
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
### for 循环
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

## 运算符
https://www.liwenzhou.com/posts/Go/03_operators/

Go 语言内置的运算符有：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 赋值运算符

### 算数运算符

运算符 | 描述
---|---
+ | 相加
- | 相减
* | 相乘
/ | 相除
% | 求余

注意： `++`(自增)和`--`(自减)在Go语言中是单独的语句，并不是运算符。

### 关系运算符


运算符 | 描述
----|---
== | 检查两个值是否相等，如果相等返回 True 否则返回 False。
!= | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。
> | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。
>= | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。
< | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。
<= | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。

### 逻辑运算符
运算符 | 描述
----|---
&& | 逻辑 AND 运算符。 如果两边的操作数都是 True，则为 True，否则为 False。
\|\| | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则为 True，否则为 False。
! | 逻辑 NOT 运算符。 如果条件为 True，则为 False，否则为 True。

### 位运算符
位运算符对整数在内存中的二进制位进行操作。

运算符 | 描述
----|---
& | 参与运算的两数各对应的二进位相与。（两位均为1才为1）
\|  | 参与运算的两数各对应的二进位相或。（两位有一个为1就为1）
^ | 参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。(两位不一样则为1)
<< | 左移n位就是乘以2的n次方。"a<<b"是把a的各二进位全部左移b位，高位丢弃，低位补0。
\>>	| 右移n位就是除以2的n次方。"a>>b"是把a的各二进位全部右移b位。


### 赋值运算符
运算符|描述
----|---
=|	简单的赋值运算符，将一个表达式的值赋给一个左值
+=|	相加后再赋值
-=|	相减后再赋值
*=	|	相乘后再赋值
/=	|	相除后再赋值
%=	|	求余后再赋值
<<=	|	左移后赋值
>>=	|	右移后赋值
&=	|	按位与后赋值
|=	|	按位或后赋值
^=	|	按位异或后赋值
## fmt
https://www.liwenzhou.com/posts/Go/go_fmt/

fmt包实现了类似C语言printf和scanf的格式化I/O。主要分为向外输出内容和获取输入内容两大部分。

### 向外输出

#### Print
Print系列函数会将内容输出到系统的标准输出，区别在于`Print`函数直接输出内容，`Printf`函数支持格式化输出字符串，`Println`函数会在输出内容的结尾添加一个换行符。

```go
func Print(a ...interface{}) (n int, err error)
func Printf(format string, a ...interface{}) (n int, err error)
func Println(a ...interface{}) (n int, err error)
```


```go
func main() {
	fmt.Print("在终端打印该信息。")
	name := "沙河小王子"
	fmt.Printf("我是：%s\n", name)
	fmt.Println("在终端打印单独一行显示")
}
```

```
在终端打印该信息。我是：沙河小王子
在终端打印单独一行显示
```

#### Fprint
`Fprint`系列函数会将内容输出到一个`io.Writer`接口类型的变量w中，我们通常用这个函数往文件中写入内容。
```go
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```
```
// 向标准输出写入内容
fmt.Fprintln(os.Stdout, "向标准输出写入内容")
fileObj, err := os.OpenFile("./xx.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
if err != nil {
	fmt.Println("打开文件出错，err:", err)
	return
}
name := "沙河小王子"
// 向打开的文件句柄中写入内容
fmt.Fprintf(fileObj, "往文件中写如信息：%s", name)
```
注意，只要满足`io.Writer`接口的类型都支持写入。



#### Sprint
`Sprint`系列函数会把传入的数据生成并返回一个字符串。

```go
func Sprint(a ...interface{}) string
func Sprintf(format string, a ...interface{}) string
func Sprintln(a ...interface{}) string
```

```go
s1 := fmt.Sprint("沙河小王子")
name := "沙河小王子"
age := 18
s2 := fmt.Sprintf("name:%s,age:%d", name, age)
s3 := fmt.Sprintln("沙河小王子")
fmt.Println(s1, s2, s3)
```
### Errorf
`Errorf`函数根据format参数生成格式化字符串并返回一个包含该字符串的错误。
```go
func Errorf(format string, a ...interface{}) error
```
```go
err := fmt.Errorf("这是一个错误")
```
#### 格式化占位符
`*printf`系列函数都支持format格式化参数，在这里我们按照占位符将被替换的变量类型划分，方便查询和记忆。

**通用占位符**

占位符|说明
---|---
%v	|值的默认格式表示
%+v	|类似%v，但输出结构体时会添加字段名
%#v	|值的Go语法表示
%T	|打印值的类型
%%	|百分号

```
fmt.Printf("%v\n", 100)
fmt.Printf("%v\n", false)
o := struct{ name string }{"小王子"}
fmt.Printf("%v\n", o)
fmt.Printf("%#v\n", o)
fmt.Printf("%T\n", o)
fmt.Printf("100%%\n")
```
```
100
false
{小王子}
struct { name string }{name:"小王子"}
struct { name string }
100%
```
**布尔型**

占位符	|说明
---|---
%t	|true或false

**整型**

占位符	|说明
---|---
%b	|表示为二进制
%c	|该值对应的unicode码值
%d	|表示为十进制
%o	|表示为八进制
%x	|表示为十六进制，使用a-f
%X	|表示为十六进制，使用A-F
%U	|表示为Unicode格式：U+1234，等价于”U+%04X”
%q	|该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示

```
n := 65
fmt.Printf("%b\n", n)
fmt.Printf("%c\n", n)
fmt.Printf("%d\n", n)
fmt.Printf("%o\n", n)
fmt.Printf("%x\n", n)
fmt.Printf("%X\n", n)
```
```
1000001
A
65
101
41
41
```

**浮点数与复数**

占位符	|说明
---|---
%b	|无小数部分、二进制指数的科学计数法，如-123456p-78
%e	|科学计数法，如-1234.456e+78
%E	|科学计数法，如-1234.456E+78
%f	|有小数部分但无指数部分，如123.456
%F	|等价于%f
%g	|根据实际情况采用%e或%f格式（以获得更简洁、准确的输出）
%G	|根据实际情况采用%E或%F格式（以获得更简洁、准确的输出）

```
f := 12.34
fmt.Printf("%b\n", f)
fmt.Printf("%e\n", f)
fmt.Printf("%E\n", f)
fmt.Printf("%f\n", f)
fmt.Printf("%g\n", f)
fmt.Printf("%G\n", f)
```
```
6946802425218990p-49
1.234000e+01
1.234000E+01
12.340000
12.34
12.34
```

**字符串和[]byte**
占位符	|说明
---|---
%s	|直接输出字符串或者[]byte
%q	|该值对应的双引号括起来的go语法字符串字面值，必要时会采用安全的转义表示
%x	|每个字节用两字符十六进制数表示（使用a-f
%X	|每个字节用两字符十六进制数表示（使用A-F）

```
s := "小王子"
fmt.Printf("%s\n", s)
fmt.Printf("%q\n", s)
fmt.Printf("%x\n", s)
fmt.Printf("%X\n", s)
```
```
小王子
"小王子"
e5b08fe78e8be5ad90
E5B08FE78E8BE5AD90
```
**指针**

占位符	|说明
---|---
%p	|表示为十六进制，并加上前导的0x
```
a := 10
fmt.Printf("%p\n", &a)
fmt.Printf("%#p\n", &a)
```
```
0xc000094000
c000094000
```

**宽度标识符**
宽度通过一个紧跟在百分号后面的十进制数指定，如果未指定宽度，则表示值时除必需之外不作填充。精度通过（可选的）宽度后跟点号后跟的十进制数指定。如果未指定精度，会使用默认精度；如果点号后没有跟数字，表示精度为0。举例如下：

占位符	|说明
---|---
%f	|默认宽度，默认精度
%9f	|宽度9，默认精度
%.2f	|默认宽度，精度2
%9.2f	|宽度9，精度2
%9.f	|宽度9，精度0

```
n := 12.34
fmt.Printf("%f\n", n)
fmt.Printf("%9f\n", n)
fmt.Printf("%.2f\n", n)
fmt.Printf("%9.2f\n", n)
fmt.Printf("%9.f\n", n)
```

```
12.340000
12.340000
12.34
    12.34
       12
```

### 获取输入
Go语言fmt包下有`fmt.Scan`、`fmt.Scanf`、`fmt.Scanln`三个函数，可以在程序运行过程中从标准输入获取用户的输入。

#### Scan
- Scan从标准输入扫描文本，读取由空白符分隔的值保存到传递给本函数的参数中，换行符视为空白符。
- 本函数返回成功扫描的数据个数和遇到的任何错误。如果读取的数据个数比提供的参数少，会返回一个错误报告原因。

```
func Scan(a ...interface{}) (n int, err error)
```
```
func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scan(&name, &age, &married)
	fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}
```
#### Scanf
- Scanf从标准输入扫描文本，根据format参数指定的格式去读取由空白符分隔的值保存到传递给本函数的参数中。
- 本函数返回成功扫描的数据个数和遇到的任何错误。

```
func Scanf(format string, a ...interface{}) (n int, err error)
```

```
func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scanf("1:%s 2:%d 3:%t", &name, &age, &married)
	fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}
```

#### Scanln
- Scanln类似Scan，它在遇到换行时才停止扫描。最后一个数据后面必须有换行或者到达结束位置。
- 本函数返回成功扫描的数据个数和遇到的任何错误。

```
func Scanln(a ...interface{}) (n int, err error)
```

```
func main() {
	var (
		name    string
		age     int
		married bool
	)
	fmt.Scanln(&name, &age, &married)
	fmt.Printf("扫描结果 name:%s age:%d married:%t \n", name, age, married)
}
```


#### bufio.NewReader

有时候我们想完整获取输入的内容，而输入的内容可能包含空格，这种情况下可以使用bufio包来实现。示例代码如下：

```
func bufioDemo() {
	reader := bufio.NewReader(os.Stdin) // 从标准输入生成读对象
	fmt.Print("请输入内容：")
	text, _ := reader.ReadString('\n') // 读到换行
	text = strings.TrimSpace(text)
	fmt.Printf("%#v\n", text)
}
```
#### Fscan系列
这几个函数功能分别类似于`fmt.Scan`、`fmt.Scanf`、`fmt.Scanln`三个函数，只不过它们不是从标准输入中读取数据而是从`io.Reader`中读取数据。

```go
func Fscan(r io.Reader, a ...interface{}) (n int, err error)
func Fscanln(r io.Reader, a ...interface{}) (n int, err error)
func Fscanf(r io.Reader, format string, a ...interface{}) (n int, err error)
```

#### Sscan系列
这几个函数功能分别类似于`fmt.Scan`、`fmt.Scanf`、`fmt.Scanln`三个函数，只不过它们不是从标准输入中读取数据而是从指定字符串中读取数据。

```
func Sscan(str string, a ...interface{}) (n int, err error)
func Sscanln(str string, a ...interface{}) (n int, err error)
func Sscanf(str string, format string, a ...interface{}) (n int, err error)
```


## 高级数据结构

### 数组
#### 数组的定义
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
#### 数组的访问

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
#### 数组赋值
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
#### 数组的遍历
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

### 切片
![image](http://note.youdao.com/yws/res/78797/B5D24E47D2C54C33A32EBB77AA796ECB)

上图中一个切片变量包含三个域，分别是`底层数组的指针`、`切片的长度 length` 和`切片的容量 capacity`。切片支持 append 操作可以将新的内容追加到底层数组，也就是填充上面的灰色格子。如果格子满了，切片就需要扩容，底层的数组就会更换。

形象一点说，切片变量是底层数组的视图，底层数组是卧室，切片变量是卧室的窗户。通过窗户我们可以看见底层数组的一部分或全部。一个卧室可以有多个窗户，不同的窗户能看到卧室的不同部分。

![image](http://note.youdao.com/yws/res/102069/08737C5625A64C33AED993468624A0F4)

![image](http://note.youdao.com/yws/res/102068/5C5C52ADC357479CAD43453891F452CE)

![image](http://note.youdao.com/yws/res/102067/8ECADEF816DA4E669221A71DA8319F23)

#### 切片的创建

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

#### 切片的初始化
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
#### 空切片
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

#### 切片的赋值

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

#### 切片的遍历

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
#### 切片的追加

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
#### 切片的域是只读的

我们刚才说切片的长度是可以变化的，为什么又说切片是只读的呢？这不是矛盾么。这是为了提醒读者注意切片追加后形成了一个新的切片变量，而老的切片变量的三个域其实并不会改变，改变的只是底层的数组。这里说的是切片的「域」是只读的，而不是说切片是只读的。切片的「域」就是组成切片变量的三个部分，分别是底层数组的指针、切片的长度和切片的容量。
#### 切割
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
### 数组变切片
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
#### copy 函数

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
#### 切片的扩容点
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
### 字典
数组切片让我们具备了可以操作一块连续内存的能力，它是对同质元素的统一管理。而字典则赋予了不连续不同类的内存变量的关联性，它表达的是一种因果关系，字典的 key 是因，字典的 value 是果。如果说数组和切片赋予了我们步行的能力，那么字典则让我们具备了跳跃的能力。

指针、数组切片和字典都是容器型变量，字典比数组切片在使用上要简单很多，但是内部结构却无比复杂。

#### 字典的创建
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

使用 make 函数创建的字典是空的，长度为零，内部没有任何元素。如果需要给字典提供初始化的元素，就需要使用另一种创建字典的方式。

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
如果你可以预知字典内部键值对的数量，那么还可以给 make 函数传递一个整数值，通知运行时提前分配好相应的内存。这样可以避免字典在长大的过程中要经历的多次扩容操作。

```go
var m = make(map[int]string, 16)
```
#### 字典的读写
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
#### 字典 key 不存在会怎样？

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
#### 字典的遍历

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
奇怪的是，Go 语言的字典没有提供诸于 keys() 和 values() 这样的方法，意味着如果你要获取 key 列表，就得自己循环一下，如下

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

## 函数
https://www.liwenzhou.com/posts/Go/09_function/
### 定义
```
func 函数名(参数)(返回值){
    函数体
}
```
```
func intSum(x int, y int) int {
	return x + y
}
```
### 可变参数
本质上，函数的可变参数是通过切片来实现的

```
func intSum3(x int, y ...int) int {
	fmt.Println(x, y)
	sum := x
	for _, v := range y {
		sum = sum + v
	}
	return sum
}
```
### 闭包
```
func main() {
	// 将匿名函数保存到变量
	add := func(x, y int) {
		fmt.Println(x + y)
	}
	add(10, 20) // 通过变量调用匿名函数

	//自执行函数：匿名函数定义完加()直接执行
	func(x, y int) {
		fmt.Println(x + y)
	}(10, 20)
}
```

### defer语句

```
func main() {
	fmt.Println("start")
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)
	fmt.Println("end")
}
```

```
start
end
3
2
1
```

**defer执行时机**


![image](http://note.youdao.com/yws/res/101037/59A91EFDBFAF433AA90C878AEBFBF7CE)

**defer经典案例**

```
func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x
}

func f2() (x int) {
	defer func() {
		x++
	}()
	return 5
}

func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x
}
func f4() (x int) {
	defer func(x int) {
		x++
	}(x)
	return 5
}
func main() {
	fmt.Println(f1())
	fmt.Println(f2())
	fmt.Println(f3())
	fmt.Println(f4())
}
```

```
5
6
5
5
```

### 内置函数

内置函数	|介绍
---|---
close	|主要用来关闭channel
len	|用来求长度，比如string、array、slice、map、channel
new	|用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针
make	|用来分配内存，主要用来分配引用类型，比如chan、map、slice
append	|用来追加元素到数组、slice中
panic和recover	|用来做错误处理