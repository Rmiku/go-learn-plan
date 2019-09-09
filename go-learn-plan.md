# Golang学习计划(2019-2020)

## 目录
[TOC]

## 一、基础

### 1.先决条件

------------

#### 1)Go语法基础

##### A.Go命令行
- go bug      // 启动错误报告
- go build    // 编译包和依赖
- go clean    // 移除对象文件和缓存文件
- go doc      // 显示包或者符号的文档
- go env      // 打印go的环境信息
- go fix      // 更新包的代码为新版本
- go fmt      // 运行gofmt对代码进行格式化
- go generate // 从processing source生成go文件
- go get      // 下载并安装包和依赖
- go install  // 编译并安装包和依赖
- go list     // 列出包或者模块列表
- go mod      // 模块的维护
- go run      // 编译并运行go程序
- go test     // 测试包
- go tool     // 运行go提供的工具
- go version  // 打印go版本
- go vet      // 报告包中可能出现的错误

##### B.常量，变量，类型，函数，包等
##### C.数组&amp;切片
##### D.指针，结构，方法
##### E.接口
##### F.协程(Goroutine)，信道，缓冲区，选择(select)和互斥锁(Mutex)
##### G.延迟(Defer)机制，错误
##### H.严重(panic)异常，恢复(Recover)

------------

#### 2)Go模组

------------

##### A.Go依赖管理工具
##### B.语义版本控制(Semantic Versioning)
##### C.版本，脚本，存储库以及其他特性

------------

#### 3)基础SQL语法

##### A.Create database, Create tables
##### B.Select, Update, Delete, Insert
##### C.Distinct, Limit, Order, Like, Group, Having, 通配符
##### D.子查询，Inner Join, Left Join, Right Join, Full Join, Union
##### E.视图
##### F.存储过程
##### G.触发器
##### H.事务管理
##### I.权限管理

------------

### 2.基础开发技能

#### 1).Git
#### 2).Http/Https
#### 3).数据结构和算法
#### 4).迭代式软件开发(Scrum),看板(Kanban)以及其他项目策略
#### 5).基本Authentication, OAuth, JWT等
#### 6).SOLID(软件设计模式), YAGNI(适可而止原则), KISS(懒人原则)
#### 7).常用库

------------

## 二、进阶

### 1.命令行界面

#### 1)Cobra
#### 2)Urfave/Cli

------------

### 2.网页框架+路由

#### 1)Echo
#### 2)Beego
#### 3)Gin
#### 4)Revel
#### 5)Chi

------------

### 3.对象关系映射(ORM)

#### 1)Gorm
#### 2)Xorm

------------

### 4.数据库

#### 1)关系型数据库

##### A.PostgreSQL
##### B.CockroachDB
##### C.SQL Server
##### D.MySQL
##### E.MariaDB

#### 2)NoSQL

##### A.MongoDB
##### B.Redis
##### C.LiteDB
##### D.Apache Cassandra
##### E.RavenDB
##### F.CouchDB

#### 3)云数据库

##### A.Azure CosmosDB
##### B.Amazon AynamoDB

#### 4)搜索引擎

##### A.ElastiSearch
##### B.Solr
##### C.Sphinx

------------

### 5.高速缓存

#### 1)GCache
#### 2)分布式缓存(Distributed Cache)

##### A.Go-Redis
##### B.GoMemcache

------------

### 6.日志管理

#### 1)日志框架(log framework)

##### A.Zap
##### B.Logrus
##### C.ZeroLog
##### D.日志管理系统(Log Management System)

###### a.Sentry.io
###### b.Loggly.com

------------

### 7.实时通讯

#### 1)Melody
#### 2)Centrifugo

------------

### 8.API客户端(API Client)

#### 1)GraphQL

##### A.Gplgen
##### B.Graphql-go

#### 2)REST

##### A.Gentleman
##### B.GRequests
##### C.Heimdall

------------

### 9.测试

#### 1)单元测试(Unit Testing)
#### 2)模拟(Mocking)

##### A.GoMock

#### 3)框架/断言

##### A.Testify
##### B.Ginkgo
##### C.GoMega
##### D.GoCheck

#### 4)行为测试(Behavior Testing)

##### A.GoDog
##### B.GoConvey
##### C.GinkGo

### 5)集成测试(Integration Testing)

##### A.Testify
##### B.GinkGo

#### 6)端对端测试(E2E Testing)

##### A.Endly
##### B.Selenium

------------

### 10.常用类库

#### 1)Validator
#### 2)Glow
#### 3)GJson
#### 4)Authbss
#### 5)Go-Underscore

------------

### 11.Go模式

#### 1)Creational
#### 2)Structrul
#### 3)Behavioral
#### 4)Synchronization
#### 5)Concurrency
#### 6)Messaging
#### 7)Stability

## 三、大神

### 1.任务调度

#### 1)Gron
#### 2)jobrunner

------------

### 2.微服务

#### 1)微服务

##### A.消息代理

###### a.RabbitMQ
###### b.Apache Kafka
###### c.Active MQ
###### d.Azure Service Bus

#### 2)框架

##### A.Go-kit
##### B.Micro
##### C.rpcx
##### D.istio

#### 3)RPC

##### A.Protocol Buffers
##### B.gRPC-Go
##### C.gRPC-gateway
##### D.Twrip

## 四、保持学习

来源：[https://github.com/Quorafind/golang-developer-roadmap-cn](https://github.com/Quorafind/golang-developer-roadmap-cn)