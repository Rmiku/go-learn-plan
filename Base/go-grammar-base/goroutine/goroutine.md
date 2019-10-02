# go 协程(Goroutine)，信道，缓冲区，选择(select)和互斥锁(Mutex)

当前go版本 go version go1.12 windows/amd64

http://c.biancheng.net/golang/concurrent/
https://www.runoob.com/go/go-concurrent.html

# 目录
[TOC]

## goroutine

Goroutine 是go的并发执行体，由 Go 运行时（runtime）管理。

### 语法格式

```
// 有名函数语法
go 函数名( 参数列表 )

// 匿名函数语法
go func( 参数列表 ){
    函数体
}( 调用参数列表 )
```
- 函数名：要调用的函数名。
- 参数列表：调用函数需要传入的参数。
- 调用参数列表：启动 goroutine 时，需要向匿名函数传递的调用参数。
- 可以是匿名函数，也可以是有名函数.

例子： 

- 有名函数启动goroutine
```
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")
    say("hello")
}
```

- 匿名函数启动goroutine
```
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go func(s string) {
        for i := 0; i < 5; i++ {
            time.Sleep(100 * time.Millisecond)
            fmt.Println(s)
        }
    } ("World")
    say("hello")
}
```

### goroutine特性

- go 的执行是非阻塞的，不会等待
- go 后面函数的返回值会被忽略
- 调度器不能保证多个 goroutine 之间的执行顺序
- 没有父子 goroutine 的概念，所有的 goroutine 都是平等的被调度执行的
- Go程序在执行时会单独为 main 函数创建一个 goroutine, 遇到关键字 go 再去创建其他的 goroutine
- Go 没有暴露 goroutine id给用户，所以不能在一个 goroutine 里显式操作另一个 goroutine, 不过 runtime 包提供了一些函数访问和设置 goroutine 的相关信息

1. func GOMAXPROCS(n int) int 
   可以用来设置或查询可以执行并发的 goroutine 数目, n > 1 代表设置 GOMAXPROCS 的值，否则表示查询 GOMAXPROCS 的值
   ```
   runtime.GOMAXPROCS(0)
   ```
2. func Goexit()
   结束当前 goroutine 的运行，结束时会调用当前 goroutine 已注册 defer，但是不会产生 panic ，所以 goroutine defer 里的 recover 调用都返回 nil
3. func Gosched()
   放弃当前调度机会，将当前 goroutine 放在队列中等下次被调度

## channel

Go的哲学：不要通过共享内存来通信，而应该通过通信来共享内存

channel 是 goroutine 之间通信和同步的重要组件, 通道是Go通过通信来共享内存的载体

### 声明通道类型

通道声明

```
var 通道变量 chan 通道类型
```
- 通道类型：通道内的数据类型。
- 通道变量：保存通道的变量。

chan 类型的空值是 nil，声明后需要配合 make 后才能使用。

单向通道声明
```
var 通道实例 chan<- 元素类型    // 只能发送通道
var 通道实例 <-chan 元素类型    // 只能接收通道
```
- 元素类型：通道包含的元素类型。
- 通道实例：声明的通道变量。

例
```
ch := make(chan int)

// 声明一个只能发送的通道类型, 并赋值为ch
var chSendOnly chan<- int = ch

//声明一个只能接收的通道类型, 并赋值为ch
var chRecvOnly <-chan int = ch
```

### 创建 channel

通道在使用前必须创建，使用 chan 关键字, 通道是有类型

- 无缓冲区通道

```
make(chan 通道类型)
```

- 有缓冲区通道

```
make(chan 通道类型, 缓冲大小)
```

- 通道分无缓冲和有缓冲的通道, Go 提供内置函数 len 和 cap, 无缓冲的通道 len 和 cap 都是0, 有缓冲的通道 len 代表没有被读取的元素数, cpa 代表整个通道容量。
- 通道还有操作符，操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。
- 通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序
- 在任何时候，同时只能有一个 goroutine 访问通道进行发送和获取数据

例子：

```
package main

import (
    "fmt"
)

func sum(s []int, c chan int) {
    var sum int
    for _, v := range s {
        sum += v
    }

    c <- sum
}

func main() {

    s := []int{1, 2, 3, 4, 5, 6}
    c := make(chan int)

    go sum(s[:3], c)
    go sum(s[3:], c)
    x, y := <- c, <- c

    fmt.Printf("%v %v", x, y)
}
```

#### 无缓冲通道

无缓冲通道指在接收前没有能力保存任何值的通道，用于在两个 goroutine 之间同步交换数据。

例子：

```
package main

import "fmt"

func main() {

    s := []int {1,2,3,4,5,6,7,8,9,10}
    ch := make(chan int)

    go func(s []int, c chan int) {
        sum := 0
        for _, v := range s {
            sum += v
        }

        fmt.Printf("%v", sum)

        c <- sum
    } (s, ch)

    <- ch
    fmt.Printf("\ngoroutine 同步")
}
```

#### 有缓冲通道

有缓冲通道是一种在被接收前能存储一个或者多个值的通道。

例子：
```
package main

import "fmt"

func main() {
    // 创建一个3个元素缓冲大小的整型通道
    ch := make(chan int, 3)
    
    // 查看当前通道的大小
    fmt.Println(len(ch))
    
    // 发送3个整型元素到通道
    ch <- 1
    ch <- 2
    ch <- 3

    // 查看当前通道的大小
    fmt.Println(len(ch))
}
```

### 操作 channel 

操作不同状态的 channel 会引发三种行为:

1. panic:
- 向已经关闭的 channel 写数据会导致 panic(最佳实践是由写入者关闭通道)
- 重复关闭 channel 会导致 panic

2. 阻塞
- 向未初始化的通道读写数据会导致当前 goroutine 永久阻塞
- 向缓冲区已满的通道写入数据会导致 goroutine 阻塞
- 通道没有数据，读取该通道会导致 goroutine 阻塞

3. 非阻塞
- 读取已经关闭的不会引起阻塞，而是会返回该通道类型的零值，可以使用 comma, ok 语法判断通道是否关闭
- 向有缓冲且没有满的通道读/写不会引发阻塞

#### 发送数据

使用操作符 <- 发送数据

```
ch<- data
```

#### 接收数据

通道接收数据特性：

- 通道的收发操作在不同的两个 goroutine 间进行
- 接收将持续阻塞直到发送方发送数据
- 每次接收一个元素

通道的数据接收一共有以下 4 种写法

- 阻塞接收数据
```
data := <-ch
```

- 非阻塞接收数据
```
data, ok := <-ch
```
data：表示接收到的数据。未接收到数据时，data 为通道类型的零值。
ok：表示是否接收到数据。

- 接收任意数据，忽略接收的数据

```
<-ch
```
执行该语句时将会发生阻塞，直到接收到数据，但接收到的数据会被忽略。这个方式实际上只是通过通道在 goroutine 间阻塞收发实现并发同步。

- 循环接收

```
for data := range ch {
    ...
}
```
通道 ch 是可以进行遍历的，遍历的结果就是接收到的数据。数据类型就是通道的数据类型。通过 for 遍历获得的变量只有一个，即 data。

#### 关闭通道

使用 close() 函数关闭通道

```
close(c)
```

## WaitGroup

sync 包提供了多个 goroutine 同步的机制，主要是通过 WaitGroup 实现的。

waitGroup 的数据结构和操作如下:
https://gowalker.org/sync#WaitGroup
```
type WaitGroup struct {
    // contains filtered or unexported fields
}

// 添加等待信号
func (wg *WaitGroup) Add(delta int) {}

// 释放等待信号
func (wg *WaitGroup) Done() {}

// 等待
func (wg *WaitGroup) Wait() {}
```

例子：

```
package main

import (
    "net/http"
    "sync"
)

var wg sync.WaitGroup

var urls = []string{
    "http://www.baidu.com",
    "http://www.sina.com",
    "http://www.qq.com",
}

func main() {

    for _, v := range urls {

        wg.Add(1)

        go func (url string)  {

            // 结束当前 goproutine 后要给 wg 计数减1, wg.Done() 等价于 wg.Add(-1)
            defer wg.Done()

            response, err := http.Get(url)
            if err == nil {
                println(response.Status)
            }
        } (v)
    }

    wg.Wait()
}
```

## Select

select 是类 UNIX 系统提供的一个多路复用系统API，Go 借用了多路复用的概念，提供了 select 关键字，用于多路监听多个通道。

### 使用select

```
select{
    case 操作1:
        响应操作1
    case 操作2:
        响应操作2
    …
    default:
        没有操作情况
}
```
- 每个 case 语句里必须是一个 IO 操作

select 可以同时响应多个通道的操作，如果其中有一个或多个通道处于可读或者可写的状态，select 会随机选取一个处理，此时是非阻塞的。吐过没有一个通道时处于可读或者可写的状态，此时的 select 是阻塞的。

例子：

```
package main

func main() {

    ch := make(chan int, 1)

    go func(ch chan int) {
        for {
            // 循环写入 0 / 1 到通道里面
            select {
                case ch<- 0:
                case ch<- 1:
            }
        }
    } (ch)
    
    for i := 0; i < 10; i++ {
        println(<-ch) // 随机输出 10个数字，值为 0 / 1
    }
}
```

## Mutex

sync 包提供了 Mutex 锁机制，Mutex 是最简单的一种锁类型，同时也比较暴力，当一个 goroutine 获得了 Mutex 后，其他 goroutine 就只能乖乖等到这个 goroutine 释放该 Mutex。

Mutex 的数据结构和操作如下:
https://gowalker.org/sync#Mutex

```
type Mutex struct {
    // contains filtered or unexported fields
}

// 锁
func (m *Mutex) Lock() {}

// 解锁
func (m *Mutex) Unlock() {}
```

例子：
```
package main

import (
	"fmt"
    "sync"
    "time"
)

var s string

var m sync.Mutex

func changeStrValue(str string) {
	m.Lock()
	defer m.Unlock()

	s = str
	fmt.Printf("%s", s)
}

func main() {

    // 确保 hello 永远在 World 前输出
	go changeStrValue("Hello ")
    go changeStrValue("World!")
    
    // 睡眠一秒确保任务执行完毕
    time.Sleep(time.Second)
}
```

## RWMutex

sync 包提供了 RWMutex 锁机制，RWMutex 是经典的单写多读模型，在读锁占用的情况下，会阻止写，但不阻止读。

- 在读锁占用的情况下，会阻止写，但不阻止读，也就是多个 goroutine 可同时获取读锁（调用 RLock() 方法
- 而写锁（调用 Lock() 方法）会阻止任何其他 goroutine（无论读和写）进来，整个锁相当于由该 goroutine 独占

Mutex 的数据结构和操作如下:
https://gowalker.org/sync#RWMutex

```
type RWMutex struct {
    // contains filtered or unexported fields
}

// 写锁
func (rw *RWMutex) Lock() {}

// 读锁
func (rw *RWMutex) RLock() {}

// RLocker返回一个Locker接口，该接口通过调用rw.RLock和rw.RUnlock来实现Lock和Unlock方法 ?
func (rw *RWMutex) RLocker() Locker {}

// 读锁解锁
func (rw *RWMutex) RUnlock() {}

// 写锁解锁
func (rw *RWMutex) Unlock() {}
```

例子：

```
package main

import (
	"fmt"
	"sync"
	"time"
)

var n int

var m sync.RWMutex

func write(num int) {
	m.Lock()
	defer m.Unlock()

	fmt.Printf("写 goroutine %d 写数据中...\n", num)
	n = num
	fmt.Printf("写 goroutine %d 写入结束，写入数据为 %d\n", num, n)
}

func read(num int) {
	m.RLock()
	defer m.RUnlock()

	fmt.Printf("读 goroutine %d 读数据中...\n", num)
	fmt.Printf("读 goroutine %d 读取结束，读取数据为 %d\n", num, n)
}

func main() {

	for i := 0; i < 5; i++ {
		go write(i + 1)
	}

	for i := 0; i < 5; i++ {
		go read(i + 1)
	}

	time.Sleep(time.Second * 5)
}

// 写 goroutine 1 写数据中...
// 写 goroutine 1 写入结束，写入数据为 1
// 读 goroutine 1 读数据中...
// 读 goroutine 1 读取结束，读取数据为 1
// 读 goroutine 4 读数据中...
// 读 goroutine 5 读数据中...
// 读 goroutine 5 读取结束，读取数据为 1
// 读 goroutine 3 读数据中...
// 读 goroutine 3 读取结束，读取数据为 1
// 读 goroutine 2 读数据中...
// 读 goroutine 2 读取结束，读取数据为 1
// 读 goroutine 4 读取结束，读取数据为 1
// 写 goroutine 3 写数据中...
// 写 goroutine 3 写入结束，写入数据为 3
// 写 goroutine 5 写数据中...
// 写 goroutine 5 写入结束，写入数据为 5
// 写 goroutine 4 写数据中...
// 写 goroutine 4 写入结束，写入数据为 4
// 写 goroutine 2 写数据中...
// 写 goroutine 2 写入结束，写入数据为 2
```

可以看出 读操作 可以并行处理，而且 读过程 中不允许写入，写操作 是阻塞的，同时只能有一个写。