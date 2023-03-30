---
title: "Swift_1"
description: Swift 开发随笔#1
date: 2023-03-29T22:46:55+08:00
image: 
math: 
license: Is this license?
categories: 
  - development
tags: 
  - Swift
---

关于不同的翻译到底要怎么把单词数据转换成对应的Anki field的问题——  
（涉及到：字典类型、单词显示View、转换函数如何组织）  
~~最终决定还是：把 `SearchResult` 抽象出来，然后在各自的 `SearchResult` 中糅合~~

并不行……Swift不支持泛型数组……  
可恶你不是现代语言吗怎么这点东西都不支持……  