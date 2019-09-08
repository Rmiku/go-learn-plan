# go命令行

当前go版本 go version go1.12 windows/amd64

 - https://golang.org/cmd/go/
 - https://www.cnblogs.com/sunsky303/p/10788982.html
 - https://www.cntofu.com/book/19/index.html
 - http://c.biancheng.net/golang/build/

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
        list        list packages or modules // 列出包或者模块列表
        mod         module maintenance // 模块的维护
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

### go doc

```
go doc [-u] [-c] [package|[package.]symbol[.methodOrField]]
```

go doc 打印附于Go语言程序(变量、常量、函数、结构体以及接口)实体上的文档

```
-all  // 显示包的所有文档

-c  // 在匹配符号时大小写敏感

-cmd  // 将main包视为常规包。否则在显示程序包的顶级文档时，将隐藏程序包主导出的符号。

-src  // 显示符号的完整源代码。将显示其声明和定义的完整Go源，例如函数定义（包括正文），类型声明或封闭const 块。因此输出可能包括未导出的细节。

-u  // 显示未导出的符号，方法和字段的文档。
```

### go env

```
go env [-json] [-u] [-w] [var ...]
```

打印Go环境信息。如果给出一个或多个变量名作为参数，则env在其自己的行上打印每个命名变量的值。

```
-json  // 以json格式打印环境变量

-u     // 取消go env -w 设置命名环境变量，并还原为默认设置

-w     // 将命名环境变量的默认设置更改为给定值, NAME = VALUE
```

### go fix

```
go fix [packages]
```

把指定 代码包 的所有 Go 语言源码文件中的旧版本代码修正为新版本的代码(调用新API的代码，以及新的语法)

要使用特定选项运行修复，请运行“go tool fix”。（命令 go fix 其实是命令 go tool fix 的简单封装）

### go fmt

```
go fmt [-n] [-x] [packages]
```

按照 Go 官方代码标准格式化代码

```
-n  // 打印将要执行的命令

-x  // 在执行时打印命令
```

### go generate

```
go generate [-run regexp] [-n] [-v] [-x] [build flags] [file.go... | packages]
```

生成由现有文件中的指令描述的运行命令。这些命令可以运行任何进程，但目的是创建或更新Go源文件。

Go generate永远不会通过go build，go get，go test等自动运行。它必须明确运行。


```
-run = ""  // 如果非空，则指定正则表达式以选择其完整原始源文本（不包括任何尾随空格和最终换行符）与表达式匹配的指令。

-v  // 在处理包时打印包和文件的名称

-n  // 打印将要执行的命令

-x  // 在执行时打印命令
```

### go get

```
go get [-d] [-t] [-u] [-v] [-insecure] [build flags] [packages]
```

获取指定的包及其依赖项添加到当前开发模块

默认情况下，get查找最新的标记发行版本

可以通过在package参数中添加@version后缀来覆盖此默认版本选择

对于存储在源控制存储库中的模块，版本后缀也可以是提交哈希，分支标识符或源控制系统已知的其他语法

版本后缀@latest显式请求给定路径命名的模块的最新次要版本,后缀@upgrade与@latest类似, 后缀@patch请求最新的补丁版本

```
-d  // 下载构建命名包所需的源代码，包括下载必要的依赖项，但不构建和安装它们

-t  // 下载构建指定包的测试所需的包

-u  // 更新包和依赖 (当-t和-u标志一起使用时，get也会更新测试依赖项)

-insecure  // 允许从存储库中提取并使用不安全的方案（如HTTP）解析自定义域

-v  // 启用详细进度和调试输出。
```

### go install

```
go install [-i] [build flags] [packages]
```

安装编译并安装导入路径命名的包。

可执行文件安装在GOBIN环境变量指定的目录中，如果未设置GOPATH环境变量，则默认为 \$GOPATH/bin 或 \$HOME/go/bin 。\$GOROOT中的可执行文件安装在 \$GOROOT/bin或 \$GOTOOLDIR而不是 \$GOBIN 中。

```
-i  // 安装包的依赖
```

### go list

```
go list [-f format] [-json] [-m] [list flags] [build flags] [packages]
```

列表列出了命名包，每行一个。最常用的标志是-f和-json，它们控制为每个包打印的输出形式。

```
-f  // 使用包模板的语法指定列表的格式

-json  // 使包数据以JSON格式打印，而不是使用模板格式。

-m  // 列出模块而不是包
```

### go mod 

```
go mod <command> [arguments]
```

提供对模块操作的访问

```
download    下载模块到本地缓存
> go mod download [-json] [modules]
> -json 将下载一系列JSON对象打印到标准输出，描述每个下载的模块（或失败）

edit        用工具或脚本编辑go.mod
> go mod edit [editing flags] [go.mod]
> Edit提供了一个命令行界面，用于编辑go.mod，主要用于工具或脚本
> -fmt   重新格式化go.mod文件而不进行其他更改
> -print 以文本格式打印最终的go.mod
> -json  以JSON格式打印最终的go.mod文件

graph       打印模块需求图
> go mod graph
> 打印模块依赖关系图

init        初始化当前目录中的新模块
> go mod init [module]
> Init初始化并将新的go.mod写入当前目录，实际上创建了一个以当前目录为根的新模块,当前目录下go.mod文件不能存在

tidy        添加缺失并删除未使用的模块
> go mod tidy [-v]
> -v  整理有关已删除模块的信息打印到标准错误

vendor      制作依赖项的副本
> go mod vendor [-v]
> -v  使供应商将出售模块和包的名称打印为标准错误

verify      验证依赖项已预期内容
> go mod verify
> 验证检查当前模块的依赖关系（存储在本地下载的源缓存中）自下载以来未被修改,如果所有模块都未修改，请验证打印“所有模块已验证”。否则，它会报告哪些模块已被更改

why         解释为什么包或模块是必需的
> go mod why [-m] [-vendor] packages...
> -m  解释为什么将参数视为模块列表并找到每个模块中任何包的路径
```

### go run

```
go run [build flags] [-exec xprog] package [arguments ...]
```

运行编译并运行Go程序，默认情况下，go run直接运行已编译的二进制文件

```
-exec  // go run 使用xprog调用二进制文件
```

### go test

```
go test [build/test flags] [packages] [build/test flags & test binary flags]
```

go test命令用于对Go语言编写的程序进行测试。这种测试是以代码包为单位的,测试源码文件是名称以“_test.go”为后缀的、内含若干测试函数的源码文件。测试函数一般是以“Test”为名称前缀并有一个类型为“testing.T”的参数声明的函数.

```
-args  // 将命令行的其余部分（-args之后的所有内容）传递给测试二进制文件，取消解释并保持不变

-c     // 生成用于运行测试的可执行文件，但不执行它

-i     // 安装/重新安装运行测试所需的依赖包，但不编译和运行测试代码

-json  // 将测试输出转换为适合自动处理的JSON

-o     // 将测试二进制文件编译为指定文件,测试仍然运行
```


### go tool

```
go tool [-n] command [args...]
```

Tool运行由参数标识的go工具命,它打印已知工具列表。

```
-n  // 打印将要执行的命令
```

### go version 

```
go version
```

打印Go版本，由runtime.Version报告

### go vet

```
go vet [-n] [-x] [-vettool prog] [build flags] [vet flags] [packages]
```

报告包中可能出现的错误

```
-n  // 打印将要执行的命令

-x  // 在执行时打印命令
```
