---
title: "Swift 开发随笔1"
description: any Identifiable cannot conform to Protocol Identifiable
date: 2023-03-29T22:46:55+08:00
image: 
math: 
license: Is this license?
categories: 
  - dev
tags: 
  - Swift
---

关于不同的翻译到底要怎么把单词数据转换成对应的Anki field的问题——  
（涉及到：字典类型、单词显示View、转换函数如何组织）  
~~最终决定还是：把 `SearchResult` 抽象出来，然后在各自的 `SearchResult` 中糅合~~

~~并不行……Swift不支持泛型数组……~~  
~~可恶你不是现代语言吗怎么这点东西都不支持……~~  

它支持的！！但是，`any SearchResult cannot conform to Protocol Identifiable`  
解决方法：只要在 View 里尽量不用涉及要遵循接口的 View 就可以了……

And protocol 里的 View 要写成 any View，因为 View 本身也是接口  
解决方法：使用 `AnyView`，是一个 Structure（怎么又来一个新东西  
（PS 既然 Swift 你都有这样的工具 struct 了，官方拓展一下 Published+Codable 又会怎么样呢……