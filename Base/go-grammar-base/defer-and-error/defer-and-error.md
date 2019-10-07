# go defer 和 错误

当前go版本 go version go1.13.1 windows/amd64

http://c.biancheng.net/view/61.html
https://segmentfault.com/a/1190000020398774

# 目录
[TOC]

## defer

Go 里提供了 defer 关键字, 可以注册多个延时调用，这些调用以先进后出(FILO)的顺序在函数返回前被执行。defer 常用于保证一些资源最终一定能够得到回收和释放。

- defer 后面必须是函数或方法的调用，不能是语句。
- defer 语句必须先注册后才能执行，如果 defer 位于 return 后，则 defer 因为没有注册不会执行。
- defer 的入参是值拷贝
- defer 的位置不当，有可能导致 panic, 一般 defer 放在错误检查语句之后

defer 的副作用: defer 会推迟资源的释放，defer 尽量不要放在循环语句里面。

例子：

- 先进后出

```
package main

func main() {

	defer func() {
		println("first defer")
	} ()

	defer func() {
		println("second defer")
	} ()

	println("this is defer test")
}

// this is defer test
// second defer
// first defer
```

- 主动调用 os.Exit() ，即使注册了 defer，也不再执行

```
package main

import (
	"os"
)

func main() {

	println("this is defer test")

	defer func() {
		println("first defer")
	}()

	os.Exit(1)
}

// this is defer test
// exit status 1
```

- defer 释放资源

```
package main

import (
	"os"
)

func main() {
	name := "E:/Go/plan/README.md"

	size := fileSize(name)

	println(size)
}

func fileSize(filename string) int64 {
	// 打开文件
	f, err := os.Open(filename)
	
    if err != nil {
        return 0
	}
	
    // 在打开资源不报错后直接调用 defer 关闭资源是一种良好的编程习惯
	defer f.Close()

	// 取文件状态信息
    info, err := f.Stat()

    if err != nil {
        f.Close()
        return 0
    }

	// 取文件大小
    size := info.Size()
   
    return size
}
```

## 错误处理

Go 语言内置错误接口类型 error, 任何类型只要实现 Error() string 方法，都可以传递 error 接口类型变量。

```
type error interface {
    Error() string
}
```
Go 语言1.13版本更新了 errors 的一些方法，下面是数据接口和类型

https://gowalker.org/errors

```
// As 判断错误链类型是否相同
它递归调用 Unwrap 并判断每一层的 err 是否类型相同，如果有任何一层 err 和传入的目标错误类型相同，则返回 true
func As(err error, target interface{}) bool

// Is 判断两个error 是否相等
// 它递归调用 Unwrap 并判断每一层的 err 是否相等，如果有任何一层 err 和传入的目标错误相等，则返回 true
func Is(err, target error) bool

// New 抛出一个Text错误
func New(text string) error

// Unwrap 拆开一个被包装的 error
func Unwrap(err error) error

```

一些基础用法

- 运用 fmt.Errorf

```
package main

import (
	"fmt"
	"errors"
)

func main() {

	err1 := errors.New("new error")
	err2 := fmt.Errorf("err2: [%w]", err1)
	err3 := fmt.Errorf("err3: [%w]", err2)
	
	fmt.Println(err3)
	fmt.Println(errors.Unwrap(err3))
	fmt.Println(errors.Unwrap(errors.Unwrap(err3)))
}

// err3: [err2: [new error]]
// err2: [new error]
// new error
```

- 自定义 struct

```
type WarpError struct {
    msg string
    err error
}

func (e *WarpError) Error() string {
    return e.msg
}

func (e *WrapError) Unwrap() error {
    return e.err
}
```

错误的最佳实践：

- 多个返回值的函数中，error通常作为函数最后一个返回值
- 如果一个函数返回 error 类型变量, 则先用 command，ok 语句处理 error != nil的异常场景
- defer 应放在 err 判断后，防止产生 panic
- 错误逐级向上传递时，错误信息应该不断地丰富和完善，而不是简单的抛出下层调用的错误