# 接口
https://www.liwenzhou.com/posts/Go/12_interface/
```
package main

import "fmt"

/*
// 接口定义
type 接口类型名 interface{
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
    …
}
*/

// Sayer 接口
type Sayer interface {
	say()
}

/*
  一个对象只要全部实现了接口中的方法，那么就实现了这个接口
  换句话说，接口就是一个需要实现的方法列表
*/

// dog 对象
type dog struct {}

// cat 对象
type cat struct {}

// dog实现了Sayer接口
func (d dog) say() {
	fmt.Println("汪汪汪")
}

// cat实现了Sayer接口
func (c cat) say() {
	fmt.Println("喵喵喵")
}

// 接口类型变量
func interfaceDemo1() {
	var sayer Sayer // 声明一个Sayer类型的变量sayer
	var mover Mover
	cat1 := cat{}   // 实例化一个cat
	dog1 := dog{}   // 实例化一个dog
	sayer = cat1    // 可以把cat实例直接赋值给sayer
	sayer.say()     // 喵喵喵
	sayer = dog1    // 可以把dog实例直接赋值给sayer
	sayer.say()     // 汪汪汪
	mover = dog1
	mover.move()
}

// 接口嵌套
type animal interface {
	Sayer
	Mover
}

// 值接收者和指针接收者实现接口的区别
type Mover interface {
	move()
}

/*
使用值接收者实现接口之后，不管是dog结构体还是结构体指针*dog类型的变量都可以赋值给该接口变量
使用指针接收者实现接口之后，只能接收结构体指针*dog类型的变量
*/

func (d dog) move() {  // 值接收者实现接口
//func (d *dog) move() {  // 指针接收者实现接口
	fmt.Println("狗会动")
}

func (c cat) move() {
	fmt.Println("猫会动")
}

// 空接口
/*
 空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口。
 空接口类型的变量可以存储任意类型的变量。
*/

func interfaceDemo2(){
	// 定义一个空接口x
	var x interface{}
	s := "Hello 沙河"
	x = s
	fmt.Printf("type:%T value:%v\n", x, x)
	i := 100
	x = i
	fmt.Printf("type:%T value:%v\n", x, x)
	b := true
	x = b
	fmt.Printf("type:%T value:%v\n", x, x)
}

/*
空接口的应用
// 空接口作为函数的参数
func show(a interface{}) {
	fmt.Printf("type:%T value:%v\n", a, a)
}
// 空接口作为map的值
var studentInfo = make(map[string]interface{})
studentInfo["name"] = "沙河娜扎"
studentInfo["age"] = 18
studentInfo["married"] = false
fmt.Println(studentInfo)
 */

func main() {
	interfaceDemo1()
	interfaceDemo2()
}
```


```
// 类型断言
var w io.Writer
w = os.Stdout
w = new(bytes.Buffer)
w = nil
```
![image](http://note.youdao.com/yws/res/108041/31CBF92876E748649A051DF8161E529A)

```
// 想要判断空接口中的值这个时候就可以使用类型断言，其语法格式：
// x.(T)

func main() {
	var x interface{}
	x = "Hello 沙河"
	v, ok := x.(string)
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("类型断言失败")
	}
}

func justifyType(x interface{}) {
	switch v := x.(type) {
	case string:
		fmt.Printf("x is a string，value is %v\n", v)
	case int:
		fmt.Printf("x is a int is %v\n", v)
	case bool:
		fmt.Printf("x is a bool is %v\n", v)
	default:
		fmt.Println("unsupport type！")
	}
}
```

关于接口需要注意的是，只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口。不要为了接口而写接口，那样只会增加不必要的抽象，导致不必要的运行时损耗
