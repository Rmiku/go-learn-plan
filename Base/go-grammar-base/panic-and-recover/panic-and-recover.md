# go 严重(panic)异常，恢复(Recover)

当前go版本 go version go1.13.1 windows/amd64

http://c.biancheng.net/view/63.html
http://c.biancheng.net/view/64.html

# 目录

[TOC]

## 严重(panic)异常 

panic 是 golang 的内置函数，用于主动抛出错误。引发 panic 有两种情况，一种是程序运行中产生的错误, 由运行时检测并抛出,一种是程序主动调用pancie。

发生 panic 后, 程序会从调用 panic 的函数为止或发生 panic 的地方立即返回, 逐层向上执行函数的 defer 语句, 然后逐层打印函数调用堆栈, 直到被 recover 捕获或运行到最外层函数而退出。

```
panic(i interface{})
```
panic 的参数是一个空接口，所以可以接收任意变量的值。

需要主动抛出 paninc 的情况

- 程序遇到无法正常执行下去的错误，主动调用 panic 函数结束程序运行
- 在调试中，通过主动调用 panic 函数时实现快速退出，panic 打印出的堆栈能够更快的定位错误

例子: 

```
package main

func main() {
    panic("crash")
}

// panic: crash
// 
// goroutine 1 [running]:
// main.main()
//         E:/Go/plan/test.go:4 +0x40
// exit status 2
```

当 panic 触发时，panic 后面的代码将不会被运行，但是在 panic 函数前面已经运行过的 defer 语句依然会发生作用。

```
package main

func main() {
	defer println("first defer")
	panic("crash")
	defer println("second defer")
}

// first defer
// panic: crash
// 
// goroutine 1 [running]:
// main.main()
//         E:/Go/plan/test.go:5 +0x7b
// exit status 2
```

## 恢复(Recover) 

recover 是 golang 的内置函数，用于捕获 panic, 阻止 panic 继续向上传递。recover 和 deder 通常在一起使用。

```
recover() interface{}
```

例子:
```
package main

import "fmt"

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	} ()
	panic("crash")
}

// crash
```

只有最后一次的 panic 调用能被捕获

```
package main

import "fmt"

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()
	defer func() {
		panic("first defer panic")
	}()
	defer func() {
		panic("second defer panic")
	}()
	panic("main defer panic")
}

// first defer panic
```
recover 只有在 defer 后面的函数体内被直接调用才能捕获 panic 终止异常, 否则会返回 nil, 异常继续向上传递。

```
// 以下情况均会捕获 panic 失败
defer recover()


defer fmt.Println(recover())


defer func() {
    func() {
        recover()
    } ()
} ()


// 以下场景均可以捕获成功
defer func() {
    recover()
} ()


defer except()

func except() {
    recover()
}

```

包中 init 函数中的 painc 只能在 init 函数里捕获，在 main 里无法被捕获，原因是 init 函数先于 main 执行。

```
package main

import "fmt"

func init() {
	panic("init defer panic")
}

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()
	panic("main defer panic")
}

// main.init.0()
//        E:/Go/plan/test.go:6 +0x40
// exit status 2
```
正常情况下:

```
package main

import "fmt"

func init() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()
	panic("init panic")
}

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()
	panic("main panic")
}

// init panic
// main panic
```
