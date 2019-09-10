# go 数据类型

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/golang/syntax/
https://www.runoob.com/go/go-tutorial.html


# 目录

[TOC]

# 类型

## 定义

Go语言的基本类型有：

- 布尔型
- 数字类型
- 字符串类型
- 派生类型

## 布尔型

只可以是常量 true 和 false

## 数字类型

### 整型

无符号范围 2^n-1
有符号范围 -2^{n-1} 至 2^{n-1}-1

- uint8  无符号 8  位整型  (0 到 255) 
- uint16 无符号 16 位整型  (0 到 65535) 
- uint32 无符号 32 位整型  (0 到 4294967295) 
- uint64 无符号 64 位整型  (0 到 18446744073709551615) 
- int8   有符号 8  位整型  ((-128 到 127) 
- int16  有符号 16 位整型  (-32768 到 32767) 
- int32  有符号 32 位整型  (-2147483648 到 2147483647) 
- int64  有符号 64 位整型  (-9223372036854775808 到 9223372036854775807) 

### 浮点型

它们的算术规范由 IEEE754 浮点数国际标准定义

- float32     IEEE-754 32位浮点型数
- float64     IEEE-754 64位浮点型数
- complex64   32 位实数和虚数 (复数类型)
- complex128  64 位实数和虚数 (复数类型)

### 其他数字类型

- byte       byte 类型是 uint8 的别名,代表了 ASCII 码的一个字符
- rune       rune 类型是 uint32 的别名,代表一个 UTF-8 字符
- int        受到编译目标平台字节长度的影响，最少占32位，32 或 64 位
- uint       与int相同
- uintptr    无符号整型，用于存放一个指针

## 字符串类型

字符串是一种值类型，且值不可变，即创建某个文本后你无法再次修改这个文本的内容；更深入地讲，字符串是字节的定长数组。

- 字符串拼接符为　+　号
- 字符串实现基于 UTF-8 编码

## 派生类型

- 指针类型（Pointer）
- 数组类型
- 结构化类型(struct)
- Channel 类型
- 函数类型
- 切片类型
- 接口类型（interface）
- Map 类型

# 常量

## 定义

```
const identifier [type] = value
```

使用关键字 const 定义，用于存储不会改变的数据。
常量是在编译时被创建，并且只能是布尔型、数字型（整数型、浮点型和复数）和字符串型

在 Go 语言中，你可以省略类型说明符 [type]，因为编译器可以根据变量的值来推断其类型。
- 显式类型定义： const b string = "abc"
- 隐式类型定义： const b = "abc"

事例：
```
//单行
const a int  = 1

//批量声明
const (
    a = 1
    b = 2
    c = 3
)

//批量声明中，如果省略初始化表达式，则表示使用前面常量的初始化表达式写法
const (
    a = 1
    b
    c = 2
    d
)

fmt.Println(a, b, c, d) // "1 1 2 2"
```

## iota 表达式

iota用于生成一组以相似规则初始化的常量。
在一个 const 声明语句中，在第一个声明的常量所在的行，iota 将会被置为 0，然后在每一个有常量声明的行加一。

```
const (
    a = iota
    b
    c
    d
    e
    f
)
fmt.Println(a, b, c, d, e, f) // "0 1 2 3 4 5"

const (
    a = 1
    b = 2
    c = iota
    d
    e
    f
)
fmt.Println(a, b, c, d, e, f) // "1 2 2 3 4 5"
```

## 无类型表达式

# 变量

## 定义

```
var identifier type
var identifier1, identifier2  type
```

声明变量的一般形式是使用 var 关键字,变量名由字母、数字、下划线组成，其中首个字符不能为数字

```
//单行
var a int

//多变量声明
var (
    a int
    b string
    c []float32
    d func() bool
    e struct {
        x int
    }
)

//省略写法
//出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误
//多个短变量声明和赋值中，至少有一个新声明的变量出现在左值中，即便其他变量名可能是重复声明的，编译器也不会报错
a := 1

a, b, c := 1, 2, 3
```

## 多重赋值

多重赋值时，变量的左值和右值按从左到右的顺序赋值。

```
var a int = 100
var b int = 200

b, a = a, b
```

## 匿名变量

匿名变量的特点是一个下画线 _ ，_ 本身就是一个特殊的标识符，被称为空白标识符。
匿名变量不占用命名空间，不会分配内存。匿名变量与匿名变量之间也不会因为多次声明而无法使用。
提示：在 Lua 等编程语言里，匿名变量也被叫做哑元变量。

```
func GetData() (int, int) {
    return 100, 200
}

a, _ := GetData()
_, b := GetData()

fmt.Println(a, b) //100, 200
```

# 函数

函数是基本的代码块，用于执行一个任务。

Go 语言最少有个 main() 函数。

## 定义

```
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

- func  函数由 func 开始声明
- function_name 函数名称，函数名和参数列表一起构成了函数签名。
- parameter list 参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
函数体：函数定义的代码集合。

## 多返回值

go语言中，函数能返回多个值

```
func swap(a int, b int) (int, int) {
    a, b = b, a
    return a, b
}

//接受返回值
c, d := swap(100, 200)

//也可以用_忽略其中一个返回值
_, e := swap(300, 400)
```

## 值传递和引用传递

- 值传递  
  在调用函数时将实际参数复制一份传递到函数中，修改时将不会影响到实际参数

```
func swap(a int, b int) (int, int) {
    a, b = b, a
    return a, b
}

var a int = 100
var b int = 200

swap(a, b) // 200, 100

fmt.Printf("%d", "%d", a, b)  //100, 200
```

- 引用传递
  在调用函数时将实际参数的地址传递到函数中，修改时将影响到实际参数

```
func swap(a *int, b *int) (int, int) {
    a, b = b, a
    return a, b
}

var a int = 100
var b int = 200

swap(&a, &b) // 200, 100

fmt.Printf("%d", "%d", a, b)  //200, 100
```

## 闭包

匿名函数就是一个闭包，闭包是可以包含自由（未绑定到特定对象）变量的代码块

```
func sum(a int, b int) int {
    return func () int {
        retrun a + b
    }
}

```

## 函数入参

函数可以作为另外一个函数的实参

```
package main
import "fmt"

// 声明一个函数类型
type cb func(int) int

func main() {
    testCallBack(1, callBack) // 我是回调，x：1
}

func testCallBack(x int, f cb) {
    f(x)
}

func callBack(x int) int {
    fmt.Printf("我是回调，x：%d\n", x)
    return x
}
```

## 方法

一个方法就是一个包含了接受者的函数，接受者可以是命名类型或者结构体类型的一个值或者是一个指针。所有给定类型的方法属于该类型的方法集。

```
func (variable_name variable_data_type) function_name() [return_type]{
   /* 函数体*/
}
```

事例：
```
package main

import "fmt"

type Sum struct {
	a int
	b int
}

func main() {
	var s Sum
	s.a = 10
	s.b = 20
	fmt.Println("两者的和 = %d", s.getSum()) // 两者的和 = 30
}

//该 method 属于 sum 类型对象中的方法
func (s Sum) getSum() int {
	//s.a 即为 sum 类型对象中的属性
	return s.a + s.b
}

```

面向对象式理解,可以理解为函数为方法(此处为PHP)

```
abstract class SumAbstract
{
	private $a;
	
	private $b;
	
    public function getSum(){}
}

class Sum extends SumAbstract
{
    private $a;
	
    private $b;
	
	public function __construct($a, $b)
	{
		$this->a = $a;
		$this->b = $b;
	}

    public function getSum()
    {
        return $this->a + $this->b;
    }
}

echo (new Sum(1, 2))->getSum();
```