# 函数
https://www.liwenzhou.com/posts/Go/09_function/
## 定义
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
## 可变参数
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
## 闭包
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

## defer语句

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

## 内置函数

内置函数	|介绍
---|---
close	|主要用来关闭channel
len	|用来求长度，比如string、array、slice、map、channel
new	|用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针
make	|用来分配内存，主要用来分配引用类型，比如chan、map、slice
append	|用来追加元素到数组、slice中
panic和recover	|用来做错误处理