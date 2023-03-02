---
title: Java基础语法笔记(二)(String)
date: 2023-02-27 17:22:55
toc: true
tags: [java]
categories:
- [编程语言, Java]
description: 本文章记录了Java基础语法，主要是 String 相关的知识点。
cover: https://cdn.pixabay.com/photo/2019/07/14/16/29/pen-4337524__480.jpg
---

# 概述

`Java` 的 `String` 与 `C++` 的 `String` 差别比较大，这里做详细介绍，挖掘一下 `String` 的用法

# `String` 类

`Java` 中 `char` 类型是采用 `Unicode` 编码，字节数大小为 2，因此 `String` 类型也是采用 `Unicode` 编码，这样能够方便地表示所有字符。

`Java5.0`  版本 之前，`String` 类型采用的是内部编码为 `UCS-2` 的定长编码，`Unicode` 协会天真的以为世界上所有的字符都能够使用 16 `bit` 表示，但事实证明 16 `bit` 数据最多能表示 65536 个字符，远远不够，因此 `Unicode` 协会最后采用了 `UTF-16` 的变长编码，有的字符可能需要 2 个 16 `bit` 数据来表示，`Java5.0` 版本之后适配了这种变化，为了兼容之前的版本，提出了一些新的概念，**码点** 和 **编码单元**

编码单元就是 `char` 类型所能表示的字节大小，码点则可能由一个或者多个编码单元组成，是实际字符所代表的值，`U+0000` ~ `U+FFFF` 范围的码点称为 **经典字符**，超过 `U+FFFF` 范围的码点称为 **代理字符**

## `String` 对象的遍历

这里列举一些常用的方法：

|     String对象方法     |              描述              |
| :--------------------: | :----------------------------: |
|       `length`()       | 返回字符串所占用的编码单元数量 |
|   `codePointCount`()   |      返回范围内码点的数量      |
|       `charAt`()       |      返回索引点的编码单元      |
| `offsetByCodePoints`() |  返回指定编码点的编码单元索引  |
|    `codePointAt`()     |  返回编码单元所对应编码点的值  |

还有两个与之相关的字符类的方法：

|      `Character` 类方法      |                             描述                             |
| :--------------------------: | :----------------------------------------------------------: |
| `isSupplementaryCodePoint`() |       判断编码点的值是否为代理字符，如果是返回 `true`        |
|       `isSurrogate`()        | 判断字符值是否是辅助字符（代理字符低位的编码单元），如果是返回 `true` |

按照 `C++` 的用法对 `String` 进行遍历会存在乱码的现象：

```java
public static void main(String[] args) {
    String s = "hello!\uD835\uDD46";
    System.out.printf("%s, 字符长度：%d\n", s, s.length());

    for (int i = 0; i < s.length(); i++) {
        System.out.printf("%c ", s.charAt(i));
    }
}
```

```tex
hello!𝕆, 字符长度：8
h e l l o ! ? ? 
```

> 出现乱码的现象是因为 `𝕆` 字符是代理字符，占两个编码单元，单独打印各个编码单元无法恢复代理字符

正确的遍历方法如下：

```java
public static void main(String[] args) {
    String s = "hello!\uD835\uDD46";

    System.out.printf("%s, 字符长度：%d\n", s, s.length());
    
    for (int i = 0; i < s.codePointCount(0, s.length()); i++) {
        int idx = s.offsetByCodePoints(0, i);
        System.out.printf("%c ", s.codePointAt(idx));
    }
}
```

```tex
hello!𝕆, 字符长度：8
h e l l o ! 𝕆 
```

# `Sring` 的组合与存储

## `String` 的子串

`String` 对象可以通过 `substring` 方法来获取字符串的子串，这个方法接收两个参数，起始位置和终止位置，以编码单元为基准

```java
    public static void main(String[] args) {
        String s = "hello!\uD835\uDD46";

        System.out.printf("%s, 字符长度：%d\n", s, s.length());

        System.out.println(s.substring(0, s.length()-2));
        System.out.println(s.substring(s.length()-2, s.length()));
    }
```

```tex
hello!𝕆, 字符长度：8
hello!
𝕆
```

## `String` 的组合

`String` 对象可与任何寄存类型和 `String` 字符串通过 `+` 来组合在一起

```java
public static void main(String[] args) {
    String s = "hello!\uD835\uDD46";

    System.out.printf("%s, 字符长度：%d\n", s, s.length());

    System.out.println(64 + s.substring(0, s.length()-2));
    System.out.println(s.substring(s.length()-2, s.length()) + 64);
}
```

```tex
hello!𝕆, 字符长度：8
64hello!
𝕆64
```

## `String` 的存储

`Java` 中 `String` 字符串是不可变字符串，不可通过修改字符串中的字符来改变字符串内容，一般都是通过 `substring` 和 `+` 方法来组合生成新的字符串

## `String` 的比较

`String` 字符串不可通过 `==` 来比较字符串的内容是否一致，必须通过 `equals` 方法来比较字符串内容，`==` 只是比较字符串地址是否一致，可将 `String` 对象看做 `char*` 指针

```java
    public static void main(String[] args) {
        String s = "hello!\uD835\uDD46";
        String str = "hello";

        System.out.printf("%s, 字符长度：%d\n", s, s.length());

        System.out.println("hello".equals(s.substring(0, 5)));
        System.out.println("hello" == s.substring(0, 5));
        System.out.println("hello" == str);
        System.out.println("hello" == s.substring(0, 2) + "llo");
    }
```

```tex
hello!𝕆, 字符长度：8
true
false
true
false
```

> 通过上面的代码可发现通过 `substring` 和 `+` 来组合的新字符串与字符串常亮的地址是不同的，而字符串常亮的地址都是相同的

`null` 是一个特殊的字符串常量，`null` 字符串变量调用 `length`() 接口会报异常，因此调用 `String` 类型对象之前必须判断字符串不为 `null`

## 构建字符串

有些时候，字符串需要由多个琐碎的字符或者字符串组成，这样频繁地构造新的字符串效率比较低，因此 `Java` 内部提供 `StringBuilder` 来帮助构造字符串

`StringBuilder` 的重要方法如下：

|                         方法                         |                             描述                             |
| :--------------------------------------------------: | :----------------------------------------------------------: |
|                    `int length`()                    |              返回构建器或缓冲器中的编码单元数量              |
|          `StringBuilder append(String str)`          |                 追加一个字符串并返回 `this`                  |
|            `StringBuilder append(char c)`            |                追加一个编码单元并返回 `this`                 |
|       `StringBuilder appendCodePoint(int cp)`        | 追加一个代码点，并将其转换为一个或者两个编码单元并返回 `this` |
|           `void setCharAt(int i, char c)`            |                将第 `i` 个编码单元设置为 `c`                 |
|    `StringBuilder insert(int offset, String str)`    |         在 `offset` 位置插入一个字符串并返回 `this`          |
|      `StringBuilder insert(int offset, char c)`      |        在 `offset` 位置插入一个编码单元并返回 `this`         |
| `StringBuilder delete(int startIndex, int endIndex)` | 删除偏移量从 `startIndex` 到 `endIndex - 1` 的编码单元并返回 `this` |
|                 `String ToString()`                  |           返回一个与构建器或缓冲器内容相同的字符串           |

示例：

```java
public static void main(String[] args) {
    StringBuilder str_builder = new StringBuilder();
    String str = "world!";
    str_builder.append("hello");
    str_builder.append(" ");
    str_builder.append(str);
    System.out.printf("StringBuilder len: %d, tex: %s\n", str_builder.length(), str_builder.toString());
}
```

```tex
StringBuilder len: 12, tex: hello world!
```

> `StringBuilder` 类是在 `JDK5.0` 引入的，它的前身是 `StringBuffer` , 与之相比， `StringBuilder` 的效率较低，但是能够在多线程中正常运行，因此在单线程中最好还是使用 `StringBuffer` 来构建字符串

