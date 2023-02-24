---
title: Java基础语法笔记(一)
date: 2023-02-24 16:35:35
toc: true
tags: [java]
categories:
- [编程语言, Java]
description: 本文章记录了Java基础语法，包括最基本的源代码、程序的输入输出方法登。
cover: https://image.photocnc.com/aoaodcom/2022-04/13/20220413075453a05e31ab045e5f1648186a88fc8de026.jpg.h700.webp
---

# 概述

本章节是 `Java` 基本语法笔记的第一章，主要介绍了一个最基本的 `Java` 程序应该包含哪些必要的元素，源代码是如何编译执行的，并且对程序输入输出的方法进行了相关的介绍。

# 最基础的源代码

## 创建源代码文件

编写代码前需要创建一个文件来报错源代码，`Java` 代码的源文件后缀名必须是 `.java`，这里我们需要创建一个名为 `HelloWorld.java` 的文件。

## 向文件中添加源代码

可向文件中添加下面的简单代码。

```java
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

虽说代码并不多，但是这些代码包含一个程序最基本的元素，分别是一下几点：

- `Java` 程序必须包含一个类，且类名必须和文件名相同
- 程序中必须包含 `main` 函数，且 `main` 函数必须是类的方法
- `main` 函数的参数必须是 `String[] args`
- `main` 函数必须被 `public`、`static` 所修饰

> 这里主要解释一下为什么 `main` 函数必须被 `public`、`static` 修饰的原因：
>
> - 被 `static` 修饰是因为 `main` 函数必须是类的方法，而不是对象的方法，这样就不用创建对象来调用 `main` 函数了
> - 被 `public` 修饰是因为 `main` 函数必须类的开放方法，不应该对调用者有所限制

##  编译源文件

在编译运行前，必须确保 `JDK` 有安装好，这是编译运行 `Java` 程序的工具

编译的目的是为将源文件翻译成字节码，节省存储空间

编译方法：

```bash
javac HelloWorld.java # 输出 HelloWorld.class 字节码文件
```

## 运行字节码程序

运行 `Java` 字节码程序都是在 `Java` 虚拟机中进行的，这使得该程序能够在任何机器上运行，只要该机器安装了虚拟机，所以 `Java` 程序的可移植性非常强

运行方法：

```bash
java HelloWorld
```

# 变量类型

`Java` 中主要包含的基础变量类型见下表：

| 变量类型 | 字节大小 |
| :------: | :------: |
|   char   |    1     |
|   byte   |    1     |
| boolean  |    1     |
|  short   |    2     |
|   int    |    4     |
|   long   |    8     |
|  float   |    4     |
|  double  |    8     |

`Java` 中基础变量的大小是不变的，这得益于 `Java` 虚拟机的存在。

# 程序输出

程序输出
