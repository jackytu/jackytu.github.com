---
layout: post
title: "the zen of golang"
description: ""
category: golang
tags: [golang]
---
{% include JB/setup %}

```go
The Go programming language is an open source project to make programmers more productive.

Go is expressive, concise, clean, and efficient. Its concurrency mechanisms make it easy to write programs that get the most out of multicore and networked machines, while its novel type system enables flexible and modular program construction. Go compiles quickly to machine code yet has the convenience of garbage collection and the power of run-time reflection. It's a fast, statically typed, compiled language that feels like a dynamically typed, interpreted language.
```

golang是由google开源的开发语言，正在逐步成为云计算领域流行的开发语言；


* 如官方介绍一样，golang的设计目标在于简洁、清晰与高效，它希望能够充分利用现代服务器的多核与网络的优势；
* golang编程语言，其编译效率非常高，且一般只需要一句go build XXX.go即可；
* golang的另外一个特性在于，它会在编译时检查荣誉的代码，比如，import却没有使用的变量以及库等等；
* golang天生就是开放的，你可以直接用go get github.com/jackytu/python-nats.git，就可以下载并导入github的代码库；
* golang的goroutine非常轻量、高效，可用于并发编程；
