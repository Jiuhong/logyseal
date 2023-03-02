---
title: Java基础语法笔记(一)(输入、输出)
date: 2023-02-24 16:35:35
toc: true
tags: [java]
categories:
- [编程语言, Java]
description: 本文章记录了Java基础语法，包括最基本的源代码、程序的输入输出方法等。
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

| 变量类型 |             字节大小              |
| :------: | :-------------------------------: |
| boolean  |                 1                 |
|   byte   |                 1                 |
|   char   | 2(`Java` 默认采用 `Unicode` 编码) |
|  short   |                 2                 |
|   int    |                 4                 |
|   long   |                 8                 |
|  float   |                 4                 |
|  double  |                 8                 |

`Java` 中基础变量的大小是不变的，这得益于 `Java` 虚拟机的存在。

# 程序输出

`Java` 中程序输出主要使用两种种方法，分别是 `System.out.println` 和 `System.out.printf`，两者的区别是后者是格式化输出，与 `C` 的 `printf` 类似， `System.out.println` 方法会自动添加换行符，如果不想换行，可使用 `System.out.print`。

测序范例：

```java
public class Output {
    public static void main(String[] args) {
        String str = "Welcome!";
        double b = 64;

        // 标准输出
        System.out.println(str);
        System.out.println(b + str);
        System.out.println(str + b);

        // 格式化输出
        System.out.printf("Tom, %s", str);
        
        // 不换行输出
        System.out.print(str);
        System.out.print(str);
    }
}
```

结果：

```tex
Welcome!
64.0Welcome!
Welcome!64.0
Tom, Welcome!Welcome!Welcome!
```

分析：

- `println` 每条输出语句都会在末尾自动加上换行符
- `b + str` 和 `str + b` 都能够自动转化为 `str`

# 程序输入

`Java` 中使用 `java.util.Scanner` 类来处理输入，该类的构造函数接受 `System.in` 的输入流，对应着键盘输入设备，也接受字符串 `String` 的输入。

`java.util.Scanner`对象有以下主要方法：

|      方法      |                    描述                    |
| :------------: | :----------------------------------------: |
|  `nextByte`()  |         读取一个 `byte` 类型的整数         |
| `nextShort`()  |        读取一个 `short` 类型的整数         |
|  `nextInt`()   |         读取一个 `int` 类型的整数          |
|  `nextLong`()  |         读取一个 `long` 类型的整数         |
| `nextFloat`()  |        读取一个 `float` 类型的整数         |
| `nextDouble`() |        读取一个 `double` 类型的整数        |
|    `next`()    | 读取一个字符串，该字符在一个空白符之前结束 |
|  `nextLine`()  |  读取一行文本（即以按下回车键为结束标志）  |

> 注意事项：
>
> - 除 `nextLine` 方法会读取掉回车键，其它方法都不会读取，因此混用 `nextLine` 和其它方法时，需要注意其它方法不会读取回车键，例如连续调用 `next` 和 `nextLine` 方法时，`nextLine` 方法获取到的是空值，因为 `next` 方法会遗留一个回车键在输入缓存中，而后调用 `nextLine` 方法会直接获取到回车键，返回空值

 错误示例：

```java
public class Input {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.print("Please input your age:");
        Byte age = input.nextByte();
        System.out.print("Please input your name:");
        String name = input.nextLine();

        System.out.printf("Your name is %s, age is %d.", name, age);
    }
}
```

输出：

```tex
Please input your age:28
Please input your name:Your name is , age is 28.
```



