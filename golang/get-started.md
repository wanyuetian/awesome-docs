
# Golang 介绍

Go语言是谷歌2009发布的第二款开源编程语言，它使程序员更具生产力。

Go语言专门针对多处理器系统应用程序的编程进行了优化，使用Go编译的程序可以媲美C或C++代码的速度，而且更加安全、支持并行进程。

# Install

[下载地址](https://golang.org/dl/)

验证安装是否成功

```
$ go version
go version go1.11.5 darwin/amd64
```

# Hello World
main.go

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```
使用下面的命令运行这个文件

```
$ go run main.go
```
编译成二进制文件后运行
```
$ go build main.go
$ ./main
```

