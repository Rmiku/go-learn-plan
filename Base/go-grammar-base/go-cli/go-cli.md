# go命令行

 - https://golang.org/cmd/go/
 - https://www.cnblogs.com/sunsky303/p/10788982.html
 - https://www.cntofu.com/book/19/index.html

# 目录
[TOC]

# go语言命令行

## 可用命令

```
go help //查看可用命令
Go is a tool for managing Go source code. 

Usage:

        go <command> [arguments]

The commands are:

        bug         start a bug report //启动错误报告
        build       compile packages and dependencies //编译包和依赖
        clean       remove object files and cached files // 移除对象文件和缓存文件
        doc         show documentation for package or symbol // 显示包或者符号的文档
        env         print Go environment information // 打印go的环境信息
        fix         update packages to use new APIs // 更新包的代码为新版本
        fmt         gofmt (reformat) package sources // 运行gofmt对代码进行格式化
        generate    generate Go files by processing source // 从processing source生成go文件
        get         download and install packages and dependencies // 下载并安装包和依赖
        install     compile and install packages and dependencies // 编译并安装包和依赖
        list        list packages or modules // 包或者模块列表
        mod         module maintenance // 模块的维修
        run         compile and run Go program // 编译并运行go程序
        test        test packages // 测试包
        tool        run specified go tool // 运行go提供的工具
        version     print Go version // 打印go版本
        vet         report likely mistakes in packages // 报告包中可能出现的错误

//下面是go help <command> ，可以打印出一些概念信息

Use "go help <command>" for more information about a command.

Additional help topics:

        buildmode   build modes //构建模式的描述
        c           calling between Go and C // go和c的相互调用
        cache       build and test caching // 构建和测试缓存
        environment environment variables // 环境变量
        filetype    file types // 文件类型
        go.mod      the go.mod file // go.mod 文件
        gopath      GOPATH environment variable // GOPATH 环境变量
        gopath-get  legacy GOPATH go get // legacy模式下运行
        goproxy     module proxy protocol // 模块代理协议
        importpath  import path syntax // 导入路径语法
        modules     modules, module versions, and more // 模块，模块版本等
        module-get  module-aware go get // module-aware下运行
        packages    package lists and patterns // 包列表和模式
        testflag    testing flags // 测试标志
        testfunc    testing functions //测试功能

Use "go help <topic>" for more information about that topic.
```

### go bug

```
go bug
```

打开默认浏览器并启动新的错误报告,该报告包含有用的系统信息

### go build 

```
go build [-o output] [-i] [build flags] [packages]
```
Build会编译导入路径命名的包及其依赖项，但不会安装结果。

```
-a  // 强制重建已经是最新的软件包
    // 无结果输出,重新生成包

-n  // 打印命令但是不运行
    // 无结果输出, 不生成包

-pn // 可以并行运行的程序数，例如构建命令或测试二进制文件。默认值是可用的CPU数
    // 无法运行

-race // 启用数据竞争检测。仅支持linux / amd64，freebsd / amd64，darwin / amd64和windows / amd64
      // 无结果输出, 生成包

-v  // 在编译时打印包的名称
    // E:\Go\plan\Base\go-grammar-base\go-cli>go build -v test.go
    // command-line-arguments

-work  // 打印临时工作目录的名称，退出时不要删除它。
       // E:\Go\plan\Base\go-grammar-base\go-cli>go build -work test.go
       // WORK=C:\Users\ADMINI~1\AppData\Local\Temp\go-build326932414

-x  // 打印命令
    // E:\Go\plan\Base\go-grammar-base\go-cli>go build -x test.go
    // WORK=C:\Users\ADMINI~1\AppData\Local\Temp\go-build456246150
    // .......
    // 打印了执行的go命令

-asmflags '[pattern =] arg list'  // 传递每个go工具asm调用的参数。

-buildmode mode  // 构建模式使用。有关更多信息，请参阅“go help buildmode”。

-compiler  // 要使用的编译器名称，如runtime.Compiler（gccgo或gc）。

...
...
...

```

### go clean

```
go clean [clean flags] [build flags] [packages]
```

执行go clean命令会删除掉执行其它命令时产生的一些文件和目录，包括

- 当前包目录下用 go build 生成的包名同名或者与Go源码文件同名的可执行文件, windows下就是 xxx.exe 文件

- 当前代码包下用 go test 生成的以包名加“.test”后缀为名的文件, windows下就是 xxx.test.exe 文件

- go clean 参数带 -i 时，则会同时删除安装当前代码包时所产生的结果文件

- 编译Go或C源码文件时留在相应目录中的 _obj 和 _test 目录，名称为 _testmain.go 、test.out、build.out 或 a.out 的文件，名称以 .5 、.6、.8 、.a、.o 或 .so 为后缀的文件

- go clean 参数带 -r 时，还包括当前代码包的所有依赖包的上述目录和文件

```
-i  // 删除 go install 产生的文件

-n  // 打印它将执行的remove命令，但不运行

-r  // 递归形式删除应用于导入路径命名的包的所有依赖

-x  // 在执行它们时打印所有remove命令

-cache      // 删除整个go构建缓存

-testcache  // 删除整个go构建缓存中的测试结果

-modcache   // 删除整个模块下载缓存，包括版本化依赖项的解压缩源代码
```
