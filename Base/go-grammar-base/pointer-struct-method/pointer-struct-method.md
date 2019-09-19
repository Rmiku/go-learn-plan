# go 指针，结构，方法

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/view/21.html
http://c.biancheng.net/golang/struct/
https://www.runoob.com/go/go-pointers.html
https://www.runoob.com/go/go-structures.html

# 目录

[TOC]

# 指针

## 定义
指针（pointer）概念在Go语言中被拆分为两个核心概念：
- 类型指针，允许对这个指针类型的数据进行修改。传递数据使用指针，而无须拷贝数据。类型指针不能进行偏移和运算。
- 切片，由指向起始元素的原始指针、元素数量和容量组成。

指针的值就是地址，指针在在 32 位机器上占用 4 个字节，在 64 位机器上占用 8 个字节，并且与它所指向的值的大小无关。

- *T 在等号左边表示指针声明, 在等号右边表示取值
- Go不支持指针的运算
- 结构体指针访问结构体字段使用 . 操作符

## 初始化指针

```
var var_name *var-type

//go 支持多级指针（指向指针的指针）
var var_name_2 **var-type
```

Go语言还提供了另外一种方法来创建指针变量

new(类型)
```
str := new(string)

*str = "ninja"

fmt.Println(*str)
```

取地址操作符 & 和取值操作符 * 是一对互补操作符， & 取出地址，* 根据地址取出地址指向的值。
```
var a int = 1
var b *int

// b 是 a 的指针地址
b = &a
// 指针进行取值操作
c := *b

fmt.Printf("%v", c) // 1
```

## 空指针

当一个指针被定义后没有分配到任何变量时，它的值为 nil。

nil 指针也称为空指针。

nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。

```
var ptr *int

fmt.Printf("ptr 的值为 : %v\n", ptr) // ptr 的值为 : <nil>
```

## 指针数组

```
a := []int{10,100,200}

var b [3]*int

for  i := 0; i < 3; i++ {
    // 赋值给指针
    b[i] = &a[i]
    fmt.Printf("a[%d] = %d\n", i, *b[i] )
}
```

## 多级指针

```
var a int
var b *int
var c **int

a = 1
b = &a
c = &b

fmt.Printf("a=%d b=%d c=%d", a, *b, **c) // a=1 b=1 c=1
```

## 指针入参

```
package main

import "fmt"

func main() {
    var a int = 100
    var b int = 200
    swap(&a, &b);

    fmt.Printf("a=%d b=%d", a, b) //a=200 b=100
}
 
func swap(x, y *int){
    *x, *y = *y, *x
}
```
