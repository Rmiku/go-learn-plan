# go 接口

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/golang/interface/

# 目录

[TOC]

# 定义

接口是一种编程规范，也是一组方法签名的集合。
Go的接口是非侵入式的，只要具体类型的方法集是接口方法集的超集吗，那这个类型就实现这个借口。
接口的默认值是 nil

## 接口声明

- 接口字面量类型

一般只有空接口才会使用 interface {}

```
interface {
    Method1
    Method2
}
```

- 接口命名类型(用type关键字声明)

可以嵌入方法集合，也可以嵌入匿名接口
接口方法命名一般以 er 结尾, 如 Writer, Stringer, Closer
接口定义中，只有方法声明没有方法实现, 且每个方法声明需要独占一行

```
type interfaceName interface {
    Method1
    Method2
}

// 参数列表和返回值列表中的参数变量名可以被忽略
type writer interface{
    Write([]byte) error
}
```
## 接口嵌套

结构体与结构体之间可以嵌套，接口与接口间也可以通过嵌套创造出新的接口。

```
package main

import "fmt"

type Reader interface {
    Read()
}

type Writer interface {
    Write()
}

type ReadWriter interface {
    Reader
    Writer
}

type File struct {
}

func (f *File) Read() {
    fmt.Println("read data")
}

func (f *File) Write() {
    fmt.Println("write data")
}

func main() {

	var f File
	var r ReadWriter

	r = &f
	r.Read()
	r.Write()
}
```

## 实现接口

实现关系在 Go语言中是隐式的(因为没有类似 implements 的关键字)，Go编译器将自动在需要的时候检查两个类型之间的实现关系

接口的实现需要遵循两条规则才能让接口可用

- **接口的方法与实现接口的类型方法格式一致**

在类型中添加与接口签名一致的方法就可以实现该方法。签名包括方法中的名称、参数列表、返回参数列表。

```
package main

import "fmt"

type Calculatorer interface {
	Sum() int
	Subtract() int
	Multiply() int
	Divide() int
}

type Number struct {
	v1 int
	v2 int
}

func (n Number) Sum() int {
	return n.v1 + n.v2
}

func (n Number) Subtract() int {
	return n.v1 - n.v2
}

func (n Number) Multiply() int {
	return n.v1 * n.v2
}

func (n Number) Divide() int {
	return n.v1 / n.v2
}

func main () {

	var ca Calculatorer
    
    // Number 结构体实现了 Sum() Subtract() Multiply() Divide()方法，此时可以认为Number继承了 Calculatorer 接口
	n := Number{v1: 6, v2: 3}
	// 因为Number继承了接口，所以直接赋值是没问题的，没实现的话会报错
    ca = n

	a := ca.Sum()
	b := ca.Subtract()
	c := ca.Multiply()
	d := ca.Divide()

	fmt.Printf("a=%d b=%d c=%d d=%d", a, b, c, d) // a=9 b=3 c=18 d=2
}
```

- **接口中所有方法均被实现**

当一个接口中有多个方法时，只有这些方法都被实现了，接口才能被正确编译并使用。

当我们在 Calculatorer 接口里新增一个 Test() 在运行一下，会报： 缺失了 Test 方法

```
type Calculatorer interface {
	Sum() int
	Subtract() int
	Multiply() int
	Divide() int
    Test() int
}

// cannot use n (type Number) as type Calculatorer in assignment: Number does not implement Calculatorer (missing Test method)
```

## 类型与接口

类型和接口之间有一对多和多对一的关系

- **一个类型可以实现多个接口**

一个类型可以同时实现多个接口，而接口间彼此独立，不知道对方的实现。

```
package main

import "fmt"

type Calculatorer interface {
	Sum() int
	Subtract() int
	Multiply() int
	Divide() int
}

type Printer interface {
	Print()
}

type Number struct {
	v1 int
	v2 int
}

func (n Number) Sum() int {
	return n.v1 + n.v2
}

func (n Number) Subtract() int {
	return n.v1 - n.v2
}

func (n Number) Multiply() int {
	return n.v1 * n.v2
}

func (n Number) Divide() int {
	return n.v1 / n.v2
}

func (n Number) Print() {
	fmt.Printf("\nv1=%d v2=%d", n.v1, n.v2)
}

func main () {

	var ca Calculatorer
	var p Printer
	n := Number{v1: 6, v2: 3}
	ca = n
	p = n

	a := ca.Sum()
	b := ca.Subtract()
	c := ca.Multiply()
	d := ca.Divide()

	fmt.Printf("a=%d b=%d c=%d d=%d", a, b, c, d) // a=9 b=3 c=18 d=2

	p.Print() // v1=6 v2=3
}
```
  
- **多个类型可以实现一个接口**

一个接口的方法，不一定需要由一个类型完全实现, 可以通过多个结构嵌入到一个结构体中拼凑起来共同实现的。

```
package main

import "fmt"

type Peopler interface {
	PrintName()
	printDeposit()
}

type stranger struct {
	Student
	Bank
}

type Student struct {
	Name string
}

type Bank struct {
	deposit int
}

func (s Student) PrintName() {
	fmt.Printf("name=%s", s.Name)
}

func (b Bank) printDeposit() {
	fmt.Printf(" deposit=%v", b.deposit)
}

func main() {
	var a Peopler = stranger{Student{Name: "Tom"}, Bank{deposit: 3000}}

	a.PrintName()
	a.printDeposit()
}
```

## 接口类型断言

```
t := i.(T)
```
i 代表接口变量，T 代表转换的目标类型，t 代表转换后的变量（可以通过断言转换为其他接口类型）。

一个类型断言表达式的语法为 i.(T)，其中 i 为一个接口值， T 为一个类型名或者类型字面表示。 类型 T 可以为任意一个非接口类型，或者一个任意接口类型。

在一个类型断言表达式 i.(T) 中， i 称为断言值， T 称为断言类型。 一个断言可能成功或者失败。

一个失败的类型断言的估值结果为断言类型的零值。



- **断言为其他类型**

```
var a interface{} = 123
b, r1 := a.(int)
fmt.Println(b, r1) // 123 true

c, r2 := a.(float64)
fmt.Println(c, r2) // 0 false
```

- **断言为接口类型**

```
package main

import "fmt"

type Peopler interface {
	PrintName()
}

type Student struct {
	Name string
}

func (s Student) PrintName() {
	fmt.Printf("%s", s.Name)
}

func main() {
	var a Peopler = Student{ Name: "Tom"}

	b, r1 := a.(Peopler)
	fmt.Printf("%s %v",b, r1) // {Tom} true
}
```
- **空接口判断类型**

空接口就是不包含任何方法的接口，所有的类型都实现了空接口，所以可以存储任何类型的值。
可以用Comma-ok断言和switch判断空接口的类型

Comma-ok断言:

```
type Student struct {
	Name string
}

func (s Student) PrintName() {
	fmt.Printf("%s", s.Name)
}

func main() {
	list := make([]interface{}, 3)
    list[0] = 1       // an int
    list[1] = "Hello" // a string
    list[2] = Student{"Tom"}

    for k, v := range list {
        if value, ok := v.(int); ok {
            fmt.Printf("list[%d] = %d is int type\n", k, value)
        } else if value, ok := v.(string); ok {
            fmt.Printf("list[%d] = %s  is string type\n", k, value)
        } else if value, ok := v.(Student); ok {
            fmt.Printf("list[%d] = %s is a Student type\n", k, value)
        } else {
            fmt.Printf("list[%d] = %s not found this tuype\n", k, value)
        }
    }
}

// list[0] = 1 is int type
// list[1] = Hello is string type
// list[2] = {Tom} is a Student type
```

switch判断
```
func main() {
	list := make([]interface{}, 3)
    list[0] = 1       // an int
    list[1] = "Hello" // a string
    list[2] = Student{"Tom"}

    for k, v := range list {
        switch value := v.(type) {
		case int:
			fmt.Printf("list[%d] = %d is int type\n", k, value)
			break;
		case string:
			fmt.Printf("list[%d] = %s  is string type\n", k, value)
			break;
		case Student:
			fmt.Printf("list[%d] = %s is a Student type\n", k, value)
			break;
		default:
			fmt.Printf("list[%d] = %s not found this tuype\n", k, value)
			break;
		}
	}
}

// list[0] = 1 is int type
// list[1] = Hello is string type
// list[2] = {Tom} is a Student type
```