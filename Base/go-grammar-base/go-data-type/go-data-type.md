# go 数据类型

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/golang/syntax/


# 目录

[TOC]

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
Go语言的基本类型有：
- bool
- string
- int、int8、int16、int32、int64
- uint、uint8、uint16、uint32、uint64、uintptr
- byte // uint8 的别名
- rune // int32 的别名 代表一个 Unicode 码
- float32、float64
- complex64、complex128

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

