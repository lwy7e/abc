---
title: Go 语言入门笔记
date: 2019-03-23 11:34:24
categories:
- Go
tags: 
- Go

---
![Go](https://hexo-lwy.oss-cn-hangzhou.aliyuncs.com/hexo/go.jpg)
工作需要入手了 Go 语言，开发使用的是 [Gin Web Framework](https://github.com/gin-gonic/gin)。

入手后发现语法和 PHP/Python/JavaScript 之流还是有很大区别的，下面对学习资料及入门笔记做简单整理。

##  简介　

* Go（又称 Golang ），由 Google 开发
* 是一种 静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言
* 语法接近 C 语言，但对于变量的声明有所不同

## 安装

安装

直接前往 [官网](https://golang.org/dl/) 下载 .pkg 完成安装。
[go中文网](https://studygolang.com/) 一般去中文网下载

### workspace
用于放置一个 go 程序员的所有 go 代码和依赖。

### 目录结构
* src：go源码文件
* pkg：package object（编译出的二进制文件）
* bin：可执行文件（编译出的二进制文件）

### GOPATH
workspace 的路径，默认为 $HOME/go。

### 设置
直接安装 .pkg 文件未设置 GOPATH，需要手动设置一下：
```
export GOPATH=$HOME/go
```
加入环境变量：

```
export PATH=$PATH:$(go env GOPATH)/bin
```

## 编辑器
折腾了好久别的编辑器还是用[jetbrains](https://www.jetbrains.com/) 的 [GoLand](https://www.jetbrains.com/go/?fromMenu) 舒服
功能齐全

## Hello, World

### 入口

程序入口为：main.main。

```
package main

func main() {
    ...
}
```
### hello.go

创建新项目：/Users/jjy/go/src/exampleProject，新建文件 hello.go：

```
package main

import "fmt"

func main() {
    fmt.Println("Hello, world!")
}
```

### 编译
在 /Users/jjy/go/src/exampleProject 下执行 go install，之后会在 $GOPATH/bin 目录下生成编译后的二进制程序。

### 执行

```
▶ /Users/jjy/go/bin/exampleProject
Hello, world!
```

## 一些特性

+ 函数多返回值
+ [并发编程](https://studygolang.com/articles/13875)
+ [错误处理](http://www.runoob.com/go/go-error-handling.html)

## 学习资料

+ [官方文档](https://go-zh.org/doc/)
+ [Go语言学习 - cyent笔记](https://cyent.github.io/golang/method/overview/)
+ [awesome-go](https://github.com/avelino/awesome-go)
+ [Go 入门指南](https://github.com/Unknwon/the-way-to-go_ZH_CN)