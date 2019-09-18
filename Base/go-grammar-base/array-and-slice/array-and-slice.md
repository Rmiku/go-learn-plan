# 数组 和 切片

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/golang/container/
https://www.runoob.com/go/go-arrays.html
https://www.runoob.com/go/go-slice.html


# 目录

[TOC]

# 数组

数组是一个由固定长度的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。因为数组的长度是固定的，因此在 Go语言中很少直接使用数组。

## 初始化数组

```
var variable_name [SIZE] variable_type
```

- variable_name：数组声明及使用时的变量名。
- SIZE：数组的元素数量。可以是一个表达式，但最终通过编译期计算的结果必须是整型数值。也就是说，元素数量不能含有到运行时才能确认大小的数值。
- variable_type：可以是任意基本类型，包括 variable_type 为数组本身。但类型为数组本身时，可以实现多维数组。

数组的每个元素都可以通过索引下标来访问，索引下标的范围是从 0 开始到数组长度减 1 的位置。内置的 len 函数将返回数组中元素的个数。

```
var a = [3]int {1, 2, 3}   // 定义数组并赋值
fmt.Println(a[0])          // 打印第一个元素    1
fmt.Println(a[len(a)-1])   // 打印最后一个元素  3


var b = [3]int {1, 2}      // [1, 2, 0] 默认情况下,数组的每个元素都被初始化为元素类型对应的零值


var c = [...]int {1, 2, 3} // 数组的长度是根据初始化值的个数来计算
fmt.Printf("%T\n", q)      // [3]int
```

## 访问数组

数组的每个元素都可以通过索引下标来访问，索引下标的范围是从 0 开始到数组长度减 1 的位置。内置的 len 函数将返回数组中元素的个数。

```
var a = [3]int {1, 2, 3}
var b int = a[2]
```

## 数组比较

数组的长度是数组类型的一个组成部分，因此 [2]int 和 [3]int 是两种不同的数组类型
所以长度相同的数组之间可以比较，但是长度不同的数组之间无法比较

```
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
fmt.Println(a == b, a == c, b == c) // "true false false"

d := [3]int{1, 2}
fmt.Println(a == d) // 编译错误：无法比较 [2]int == [3]int
```

## 遍历数组

```
a := [3]string{"一", "二", "三"}

for k, v := range a {
    fmt.Println(k, v)
}

//0 一
//1 二
//2 三

```

## 多维数组

Go语言中数组本身只有一个维度，不过可以组合多个数组创建多维数组。

```
var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type
```

例如:

```
var a [2][3][4] int
```

### 二维数组

二维数组是最简单的多维数组，二维数组本质上是由一维数组组成的。
二维数组中的元素可通过 a[ i ][ j ] 来访问。

```
var arrayName [ x ][ y ] variable_type
```

具体分析：
<image src="./array-1.png">


#### 初始化二维数组

```
a = [3][4]int{  
 {0, 1, 2, 3} ,   　// 第一行索引为 0
 {4, 5, 6, 7} ,   　// 第二行索引为 0
 {8, 9, 10, 11},    // 第三行索引为 0
}
```

#### 访问二维数组

二维数组通过指定坐标来访问，如数组中的行索引与列索引

```

var a = [5][2]int{ {0,0}, {1,2}, {2,4}, {3,6},{4,8}}
var i, j int


for  i = 0; i < 5; i++ {
    for j = 0; j < 2; j++ {
        fmt.Printf("a[%d][%d] = %d\n", i,j, a[i][j] )
    }
}


//a[0][0] = 0
//a[0][1] = 0
//a[1][0] = 1
//a[1][1] = 2
//a[2][0] = 2
//a[2][1] = 4
//a[3][0] = 3
//a[3][1] = 6
//a[4][0] = 4
//a[4][1] = 8
```

# 切片

切片（slice）是对数组一个连续片段的引用，切片是一个引用类型。
切片默认指向一段连续内存区域，可以是数组，也可以是切片本身。

## 定义切片

```
var identifier []type

//或者使用make函数
var slice1 []type = make([]type,  len)

//也可以简写为
slice1 := make([]type, len)
```
make函数事例
- T为类型 
- length为切片初始长度,
- capacity为切片容量(可选),这个值设定后不影响 size,只是能提前分配空间，降低多次分配空间造成的性能问题

```
make([]T, length, capacity)
```

## 切片初始化

```
//从数组或切片生成新的切片
//slice [开始位置:结束位置]

var a  = [3]int {1, 2, 3}

fmt.Println(a, a[1:2]) //[1 2 3] [2]
```
从数组或切片生成新的切片拥有如下特性：
- 取出的元素数量为：结束位置-开始位置。
- 取出元素不包含结束位置对应的索引，切片最后一个元素使用 slice[len(slice)] 获取。
- 当缺省开始位置时，表示从连续区域开头到结束位置(slice[:N])。
- 当缺省结束位置时，表示从开始位置到整个连续区域末尾(slice[N:])。
- 两者同时缺省时，与切片本身等效。(slice[:])
- 两者同时为0时，等效于空切片，一般用于切片复位(slice[0:0] = [])。

根据索引位置取切片 slice 元素值时，取值范围是（0～len(slice)-1），超界会报运行时错误

## len() 和 cap()

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

```
var a  = [3]int {1, 2, 3}

fmt.Printf("len=%d cap=%d slice=%v\n", len(a), cap(a), a) //len=3 cap=3 slice=[1 2 3]
```

## 空切片(nil)

一个切片在未初始化之前默认为 nil，长度为 0

```
var a []int // a = nil

fmt.Printf("len=%d cap=%d slice=%v\n", len(a), cap(a), a) //len=0 cap=0 slice=[]
```

## append()

append() 可以为切片动态添加元素
不过要注意的是，在容量不足的情况下， append 的操作会导致重新分配内存（扩容）,可能导致巨大的内存分配和复制数据代价.
且切片在扩容时，容量的扩展规律按容量的 2 倍数扩充

```
var numbers []int

for i := 0; i < 10; i++ {
    numbers = append(numbers, i)
    fmt.Printf("len: %d  cap: %d pointer: %p\n", len(numbers), cap(numbers), numbers)
}

//len: 1  cap: 1 pointer: 0xc000010098
//len: 2  cap: 2 pointer: 0xc0000100f0
//len: 3  cap: 4 pointer: 0xc00000a3e0
//len: 4  cap: 4 pointer: 0xc00000a3e0
//len: 5  cap: 8 pointer: 0xc00000e1c0
//len: 6  cap: 8 pointer: 0xc00000e1c0
//len: 7  cap: 8 pointer: 0xc00000e1c0
//len: 8  cap: 8 pointer: 0xc00000e1c0
//len: 9  cap: 16 pointer: 0xc000070080
//len: 10  cap: 16 pointer: 0xc000070080
```

还可以在切片头添加数据，在开头一般都会导致内存的重新分配，而且会导致已有的元素全部复制 1 次。因此，从切片的开头添加元素的性能一般要比从尾部追加元素的性能差很多。

```
var a = []int {1,2,3}

a = append([]int{0}, a...) // 在开头添加1个元素  [0, 1, 2, 3]
a = append([]int{-3,-2,-1}, a...) // 在开头添加1个切片  [-3, -2, -1, 1, 2, 3]
```

PS: ...是go的语法糖
- 用于函数有多个不定参数的情况，可以接受多个不确定数量的参数
```
func test1(args ...string) {
    for _, v:= range args{
        fmt.Println(v)
    }
}
```
- 用于打散slice进行传递
```
var strSlice = []string{"qwr","234","yui","cvbc"}

test1(strSlice...) //切片被打散传入
```

## copy()

copy() 可以将一个数组切片复制到另一个数组切片。如果加入的两个数组切片不一样大，就会按其中较小的那个数组切片的元素个数进行复制.

```
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}

copy(slice2, slice1) // 只会复制slice1的前3个元素到slice2中 slice2 =  [1, 2, 3]
copy(slice1, slice2) // 只会复制slice2的3个元素到slice1的前3个位置 slice1 = [5, 4, 3, 4, 5]
```
copy 函数将返回成功复制的元素的个数，等于两个 slice 中较小的长度，所以我们不用担心覆盖会超出目标 slice 的范围

## 切片删除

Go语言并没有对删除切片元素提供专用的语法或者接口, 需要使用切片本身的特性来删除元素
Go语言中切片删除元素的本质是：以被删除元素为分界点，将前后两个部分的内存重新连接起来

- 从开头位置删除
```
a = []int{1, 2, 3}
a = a[1:] // 删除开头1个元素
a = a[N:] // 删除开头N个元素

//可以用 append/copy 原地完成（所谓原地完成是指在原有的切片数据对应的内存区间内完成，不会导致内存空间结构的变化）
a = append(a[:0], a[1:]...) // 删除开头1个元素
a = append(a[:0], a[N:]...) // 删除开头N个元素
a = a[:copy(a, a[1:])] // 删除开头1个元素
a = a[:copy(a, a[N:])] // 删除开头N个元素
```
- 从中间位置删除
删除开头的元素和删除尾部的元素都可以认为是删除中间元素操作的特殊情况
```
a = []int{1, 2, 3}
a = append(a[:i], a[i+1:]...) // 删除中间1个元素
a = append(a[:i], a[i+N:]...) // 删除中间N个元素
a = a[:i+copy(a[i:], a[i+1:])] // 删除中间1个元素
a = a[:i+copy(a[i:], a[i+N:])] // 删除中间N个元素
```
- 从尾部删除(最快)

```
a = []int{1, 2, 3}
a = a[:len(a)-1] // 删除尾部1个元素
a = a[:len(a)-N] // 删除尾部N个元素
```

<div style="text-align:center" >
    <img src="./slice-1.jpg"/>
</div>

## 迭代 range

可以用关键字range，它配合关键字 for 来迭代切片里的元素

```
slice := []int{10, 20, 30, 40}


for index, value := range slice {
    fmt.Printf("Index: %d Value: %d\n", index, value)
}

//Index: 0 Value: 10
//Index: 1 Value: 20
//Index: 2 Value: 30
//Index: 3 Value: 40
```

range 只是创建了每个元素的副本，而不是直接返回对该元素的引用。

## 多维切片

切片和数组一样也是一维的，不过可以组合多个切片形成多维切片

```
// 创建一个整型切片的二维切片
slice := [][]int{{10}, {100, 200}} // [[10] [100 200]]

// 可以通过append添加切片内容
slice[0] = append(slice[0], 20) // [[10 20] [100 200]]
```