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

# 结构

通过用自定义的方式形成新的类型，结构体是由一系列具有相同类型或不同类型的数据构成的数据集合。

Go 语言中的类型可以被实例化，使用new或&构造的类型实例的类型是类型的指针。

结构体成员是由一系列的成员变量构成，这些成员变量也被称为“字段”
- 字段拥有自己的类型和值
- 字段名在同一结构体里必须唯一
- 字段的类型也可以是结构体，甚至是字段所在结构体的类型

## 定义结构体

```
type name struct {
    field1 type1
    field2 type2
    …
}
```
例：
```
type Books struct {
    id int
    title string
    author string
    subject string
}

// 也可以写成单行
type Books struct {
    id int, title, author, subject string
}

```

结构体的定义只是一种内存布局的描述，只有当结构体实例化时，才会真正地分配内存。

一旦定义了结构体类型，它就能用于变量的声明

```
// 根据顺序赋值
book1 := Books {1, "活着", "余华", "小说文学"}

//根据字段名称赋值
book2 := Books {id: 2, title: "平凡的世界", author: "路遥", subject: "小说文学"}
```

## 作为函数参数

结构体类型的值可以作为参数传递给函数或者作为函数的返回值。

```
func main() {
    book1 := Books {1, "活着", "余华", "小说文学"}
	printBook(book1) // {1, "活着", "余华", "小说文学"}
	editBook(&book1) 
	printBook(book1) // {1, "活着", "测试余华", "小说文学"}
	book2 := editBook2(book1)
	printBook(book2) // {2, "许三观卖血记", "余华", "小说文学"}
}

func printBook(book Books) {
	fmt.Printf("Book book_id : %d\n", book.id);
	fmt.Printf("Book title : %s\n", book.title);
	fmt.Printf("Book author : %s\n", book.author);
	fmt.Printf("Book subject : %s\n", book.subject);
	fmt.Printf("\n")
}

// 传递指针
func editBook(book *Books) {
	book.author = "测试余华"
}

// 作为返回值
func editBook2(book Books) Books {
	book.id = 2
	book.title = "许三观卖血记"
	book.author = "余华"
	return book
}
```

## 匿名函数结构体

```
// 匿名结构体
func main() {
	book1 := struct {
		title string
		author string
	} {
		"活着",
		"余华",
	}

	printBooksType(book1)
}

func printBooksType(book struct {
    title string
    author string
}) {
    fmt.Printf("%v\n", book)
}
```

### 初始化内嵌匿名结构体

可以在定义的时候嵌入匿名结构体，不过在初始化结构时需要再次声明结构才能赋予数据

```

type People struct {
	Name string
	Attr struct {
		Height, Weight, Sex string
	}
}


func main () {

	people1 := People {
		Name: "Tom",
		Attr: struct {
			Height string
			Weight string
			Sex string
		} {
			"170cm",
			"60kg",
			"男",
		},
	}

	fmt.Printf("%v", people1) // {Tom {170cm 60kg 男}}
}

```

## 用作构造函数

Go语言的类型或结构体没有构造函数的功能。但是可以使用结构体初始化的过程来模拟实现构造函数。

### 模拟构造函数重载

Go里没有函数重载，可以用不同的名称代表类的构造过程
此处根据不同的cat属性，实例化了两个不同的cat类

```
package main

import "fmt"

type Cat struct {
    Color string
    Name  string
}

func main () {
	cat1 := NewCatByColor("yellow")
	cat2 := NewCatByName("tom")

	fmt.Printf("%v\n", cat1) // &{yellow }
	fmt.Printf("%v\n", cat2) // &{ tom}
}

func NewCatByName(name string) *Cat {
    return &Cat{
        Name: name,
    }
}

func NewCatByColor(color string) *Cat {
    return &Cat{
        Color: color,
    }
}
```

### 模拟父级构造调用

一个结构体可以嵌入另一个结构体中,可以达到实例化本身和基类的目的

Cat 结构体类似于面向对象中的“基类”。BlackCat 嵌入 Cat 结构体，类似于面向对象中的“派生”。实例化时，BlackCat 中的 Cat 也会一并被实例化。

```
package main

import "fmt"

type Cat struct {
    Color string
    Name  string
}

type BlackCat struct {
    Cat 
}

func main () {
	cat := NewCatByColor("yellow")

	fmt.Printf("%v\n", cat) // &{{yellow }}
}

func NewCat(name string) *Cat {
    return &Cat{
        Name: name,
    }
}

func NewBlackCat(color string) *BlackCat {
    cat := &BlackCat {}
    cat.Color = color
    return cat
}
```