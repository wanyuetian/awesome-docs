# 包

https://www.liwenzhou.com/posts/Go/11_package/

```golang
/*
在工程化的Go语言开发项目中，Go语言的源码复用是建立在包（package）基础之上的。本文介绍了Go语言中如何定义包、如何导出包的内容及如何导入其他包。

包（package）是多个Go源码的集合，是一种高级的代码复用方案，Go语言为我们提供了很多内置包，如fmt、os、io等。

我们还可以根据自己的需要创建自己的包。一个包可以简单理解为一个存放.go文件的文件夹。 该文件夹下面的所有go文件都要在代码的第一行添加如下代码，声明该文件归属的包。
package 包名
注意事项：
一个文件夹下面直接包含的文件只能归属一个package，同样一个package的文件不能在多个文件夹下。
包名可以不和文件夹的名字一样，包名不能包含 - 符号。
包名为main的包为应用程序的入口包，这种包编译后会得到一个可执行文件，而编译不包含main包的源代码则不会得到可执行文件。
*/
/*
如果想在一个包中引用另外一个包里的标识符（如变量、常量、类型、函数等）时，该标识符必须是对外可见的（public）。在Go语言中只需要将标识符的首字母大写就可以让标识符对外可见了。

举个例子， 我们定义一个包名为pkg2的包，代码如下：
*/
/*
package pkg2

import "fmt"

// 包变量可见性

var a = 100 // 首字母小写，外部包不可见，只能在当前包内使用

// 首字母大写外部包可见，可在其他包中使用
const Mode = 1

type person struct { // 首字母小写，外部包不可见，只能在当前包内使用
	name string
}

// 首字母大写，外部包可见，可在其他包中使用
func Add(x, y int) int {
	return x + y
}

func age() { // 首字母小写，外部包不可见，只能在当前包内使用
	var Age = 18 // 函数局部变量，外部包不可见，只能在当前函数内使用
	fmt.Println(Age)
}
*/

/*
结构体中的字段名和接口中的方法名如果首字母都是大写，外部包可以访问这些字段和方法。例如：
*/

/*
type Student struct {
	Name  string //可在包外访问的方法
	class string //仅限包内访问的字段
}

type Payer interface {
	init() //仅限包内访问的方法
	Pay()  //可在包外访问的方法
}
*/

/*
包的导入
import "包的路径"
注意事项：
import导入语句通常放在文件开头包声明语句的下面。
导入的包名需要使用双引号包裹起来。
包名是从$GOPATH/src/后开始计算的，使用/进行路径分隔。
Go语言中禁止循环导入包。
*/

/*
单行导入
单行导入的格式如下：
import "包1"
import "包2"

多行导入
多行导入的格式如下：
import (
    "包1"
    "包2"
)
*/

/*
自定义包名
在导入包名的时候，我们还可以为导入的包设置别名。通常用于导入的包名太长或者导入的包名冲突的情况。具体语法格式如下：

import 别名 "包的路径"

单行导入方式定义别名：
import "fmt"
import m "github.com/Q1mi/studygo/pkg_test"

func main() {
	fmt.Println(m.Add(100, 200))
	fmt.Println(m.Mode)
}

多行导入方式定义别名：
import (
    "fmt"
    m "github.com/Q1mi/studygo/pkg_test"
 )

func main() {
	fmt.Println(m.Add(100, 200))
	fmt.Println(m.Mode)
}
*/

/*
匿名导入包

如果只希望导入包，而不使用包内部的数据时，可以使用匿名导入包。具体的格式如下：

import _ "包的路径"

匿名导入的包与其他方式导入的包一样都会被编译到可执行文件中。
*/
```
```
/*
init()函数介绍
在Go语言程序执行时导入包语句会自动触发包内部init()函数的调用。需要注意的是： init()函数没有参数也没有返回值。 init()函数在程序运行时自动被调用执行，不能在代码中主动调用它。

包初始化执行的顺序如下图所示：
*/
```
![image](http://note.youdao.com/yws/res/108468/D08160EC566043F781257E7F7B79335E)

```golang
/*
init()函数执行顺序
Go语言包会从main包开始检查其导入的所有包，每个包中又可能导入了其他的包。Go编译器由此构建出一个树状的包引用关系，再根据引用顺序决定编译顺序，依次编译这些包的代码。

在运行时，被最后导入的包会最先初始化并调用其init()函数， 如下图示：
 */
```
![image](http://note.youdao.com/yws/res/108467/3214849FB91D4CB7865E87AE82E66ABC)


# 文件操作
https://www.liwenzhou.com/posts/Go/go_file/

## 打开和关闭文件

```
package main

/*
打开和关闭文件
 */

import (
	"fmt"
	"os"
)

// os.Open()函数能够打开一个文件，返回一个*File和一个err。对得到的文件实例调用close()方法能够关闭文件

func main() {
	// 只读方式打开当前目录下的main.go文件
	file, err := os.Open("./test.txt")

	if err != nil {
		fmt.Println("Open file failed, err:", err)
		return
	}

	// 关闭文件
	defer file.Close()
}
```

## 读取文件

```
package main

/*
  读取文件  func (f *File) Read(b []byte) (n int, err error)
 */

import (
	"fmt"
	"io"
	"os"
)


// 循环读取
func readAll(file *os.File) (content []byte) {

	var tmp = make([]byte, 128)
	for{
		n, err := file.Read(tmp)  // 它接收一个字节切片，返回读取的字节数和可能的具体错误，读到文件末尾时会返回0和io.EOF
		if err == io.EOF {
			return
		}
		if err != nil {
			fmt.Println("read file failed, err:", err)
			return
		}
		content = append(content, tmp[:n]...)
	}
	return
}


func main(){
	file, err := os.Open("./test.txt")

	if err != nil {
		fmt.Println("Open file failed, err:", err)
		return
	}

	defer file.Close()

	//// 使用Read方法读取数据
	//var tmp = make([]byte, 128)
	//n, err := file.Read(tmp)  // 它接收一个字节切片，返回读取的字节数和可能的具体错误，读到文件末尾时会返回0和io.EOF
	//if err == io.EOF {
	//	fmt.Println("文件读完了")
	//	return
	//}
	//if err != nil {
	//	fmt.Println("read file failed, err:", err)
	//	return
	//}
	//fmt.Printf("读取了%d字节数据\n", n)
	//fmt.Println(string(tmp[:n]))

	// 循环读取
	content := readAll(file)
	fmt.Println(string(content[:]))
}

```
### bufio读取文件

```
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

/*
  bufio读取文件
  bufio是在File的基础上封装了一层API，支持更多的功能
 */

func main(){
	file, err := os.Open("./test.txt")

	if err != nil {
		fmt.Println("Open file failed, err:", err)
		return
	}

	defer file.Close()

	reader := bufio.NewReader(file)
	for {
		line, err := reader.ReadString('\n')  // 注意是字符
		if err == io.EOF {
			if len(line) != 0{
				fmt.Print(line)
			}
			fmt.Println("文件读完了")
			break
		}
		if err != nil {
			fmt.Println("read file failed, err:", err)
			return
		}
		fmt.Print(line)
	}
}
```
## ioutil 读取整个文件
```
package main

import (
	"fmt"
	"io/ioutil"
)

/*
ioutil 读取整个文件
 */

func main() {
	content, err := ioutil.ReadFile("./test.txt")
	if err != nil {
		fmt.Println("read file failed, err:", err)
		return
	}
	fmt.Println(string(content))
}
```
## 文件写入
```
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

/*
文件写入操作
os.OpenFile() 函数能够以指定模式打开文件，从而实现文件写入相关功能

func OpenFile(name string, flag int, perm FileMode) (*File, error)
name: 要打开的文件名
flag: 打开文件的模式，模式有以下几种
os.O_WRONLY	只写
os.O_CREATE	创建文件
os.O_RDONLY	只读
os.O_RDWR	读写
os.O_TRUNC	清空
os.O_APPEND	追加
perm: 文件权限
*/



func main(){
	file, err := os.OpenFile("./write.txt", os.O_CREATE|os.O_WRONLY, 0644)
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()

	// Write 和 WriteString
	str := "Hello 世界"
	file.Write([]byte(str))  // 写入字节切片数据
	file.WriteString(str)  // 直接写入字符串数据

	// ioutil.WriteFile
	err = ioutil.WriteFile("./ioutil.txt", []byte(str), 0644)
	if err != nil{
		fmt.Println("write file failed, err:", err)
		return
	}
}

```

## io.Copy 拷贝文件

```
package main

import (
	"fmt"
	"io"
	"os"
)

/*
使用 io.Copy实现一个拷贝文件函数
 */

func CopyFile(dstName, srcName string)(written int64, err error){
	// 以读方式打开源文件
	src, err := os.Open(srcName)
	if err != nil {
		fmt.Printf("open %s failed, err: %v\n", srcName, err)
		return
	}
	defer src.Close()
	// 以写|创建的方式打开目标文件
	dst, err := os.OpenFile(dstName, os.O_CREATE|os.O_WRONLY, 0644)
	if err != nil {
		fmt.Println("open %s failed, err: %v\n", dstName, err)
	}
	defer dst.Close()

	return io.Copy(dst, src)  // 调用io.Copy拷贝内容
}

func main() {
	_, err := CopyFile("copy.txt", "test.txt")
	if err != nil {
		fmt.Println("copy file failed, err:", err)
		return
	}
	fmt.Println("copy done!")
}

```

## 实现cat命令功能

```
package main

import (
	"bufio"
	"flag"
	"fmt"
	"io"
	"os"
)

/*
实现cat命令功能
 */

func cat(r *bufio.Reader) {
	for {
		buf, err := r.ReadBytes('\n')  // 注意是字符
		if err == io.EOF {
			break
		}
		fmt.Fprintf(os.Stdout, "%s", buf)
	}
}


func main() {
	flag.Parse()  // 解析命令行参数
	if flag.NArg() == 0 {
		// 如果没有参数默认从标准输入读取内容
		cat(bufio.NewReader(os.Stdin))
	}
	// 依次读取每个指定文件的内容并打印到终端
	for i := 0; i < flag.NArg(); i++ {
		f, err := os.Open(flag.Arg(i))
		if err != nil {
			fmt.Fprintf(os.Stdout, "reading from %s failed, err:%v\n", flag.Arg(i), err)
			continue
		}
		cat(bufio.NewReader(f))
	}
}
```


# time
https://www.liwenzhou.com/posts/Go/go_time/

```
package main

import (
	"fmt"
	"time"
)
/* 时间间隔
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
*/

// 时间类型
func timeDemo(){
	now := time.Now()  // 获取当前时间
	fmt.Printf("Current time: %v\n", now)
	year := now.Year()
	month := now.Month()
	day := now.Day()
	hour := now.Hour()
	minute := now.Minute()
	second := now.Second()
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}

// 当前时间戳
func timestampDemo() {
	now := time.Now()  // 获取当前时间
	timestamp1 := now.Unix()
	timestamp2 := now.UnixNano()
	fmt.Printf("current timestamp1:%v\n", timestamp1)
	fmt.Printf("current timestamp2:%v\n", timestamp2)
}

// 时间戳转换
func timestampDemo2(timestamp int64) {
	timeObj := time.Unix(timestamp, 0) //将时间戳转为时间格式
	fmt.Println(timeObj)
	year := timeObj.Year()     //年
	month := timeObj.Month()   //月
	day := timeObj.Day()       //日
	hour := timeObj.Hour()     //小时
	minute := timeObj.Minute() //分钟
	second := timeObj.Second() //秒
	fmt.Printf("%d-%02d-%02d %02d:%02d:%02d\n", year, month, day, hour, minute, second)
}

// 时间操作
func timeOP(){
	now := time.Now()
	// Add
	later := now.Add(time.Hour)
	fmt.Printf("Later time: %v\n", later)
	// Sub
	duration := later.Sub(later)
	fmt.Printf("Duration: %v\n",duration)
	// Equal
	isEqual := now.Equal(later)
	fmt.Printf("now is equal later: %v\n",isEqual)
	// Before
	isBefore := now.Before(later)
	fmt.Printf("now is before later: %v\n",isBefore)
	// After
	isAfter := now.After(later)
	fmt.Printf("now is after later: %v\n",isAfter)
}

// 定时器
func tickDemo()  {
	ticker := time.Tick(time.Second)
	for i := range ticker {
		fmt.Println(i)
	}
}

// 时间格式化
func formatDemo() {
	now := time.Now()
	// 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
	// 24小时制
	fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
	// 12小时制
	fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
	fmt.Println(now.Format("2006/01/02 15:04"))
	fmt.Println(now.Format("15:04 2006/01/02"))
	fmt.Println(now.Format("2006/01/02"))
}

func formatFromString(){
	now := time.Now()
	fmt.Println(now)
	// 加载时区
	loc, err := time.LoadLocation("Asia/Shanghai")
	if err != nil {
		fmt.Println(err)
		return
	}
	// 按照指定时区和指定格式解析字符串时间
	timeObj, err := time.ParseInLocation("2006/01/02 15:04:05", "2019/08/04 14:15:20", loc)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(timeObj)
}


func main(){
	timeDemo()
	timestampDemo()
	timestampDemo2(time.Now().Unix())
	timeOP()
	//tickDemo()
	formatDemo()
	formatFromString()
}
```