---
title: "Swift 开发随笔2"
description: 好困哦。
date: 2023-04-01T15:42:32+08:00
categories: dev
tags:
  - Swift
---

终于做完字段转换了……解决方案就是，传一个 converter 进 AddNote 的 View 里，感觉有点像 delegate 的想法呢  
~~（要变成 Apple 的形状了，乌乌）~~  
到这里，这个 App 终于开始实用起来了  

然后就是为了解决 Published 和 Codable 不能共存的问题，在 StackOverflow 上找到了[惊为天人的代码](https://stackoverflow.com/a/71464346)：

```swift
//Published+Value.swift
private class PublishedWrapper<T> {
    @Published private(set) var value: T

    init(_ value: Published<T>) {
        _value = value
    }
}

extension Published {
    var unofficialValue: Value {
        PublishedWrapper(self).value
    }
}

//Published+Codable.swift
extension Published: Decodable where Value: Decodable {
    public init(from decoder: Decoder) throws {
        self.init(wrappedValue: try .init(from: decoder))
    }
}

extension Published: Encodable where Value: Encodable {
    public func encode(to encoder: Encoder) throws {
        try unofficialValue.encode(to: encoder)
    }
}
```

好厉害……感觉自己只会用东西，别人真的在造东西……  

好困……有人为了测试 App（嗯）~~看小说~~看到了 7点半  
（确实要比系统词典好用，但是没有句子翻译很麻烦……）  
但是 knmf 真的很好磕！  